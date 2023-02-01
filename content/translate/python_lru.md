---
date: 2022-10-22
title: 不要用 'functools.lru_cache' 装饰一个实例方法
author: Jn
series: Python
tags:
  - Python
---

原文链接: https://rednafi.github.io/reflections/dont-wrap-instance-methods-with-functoolslru_cache-decorator-in-python.html


最近当我想加速一个实例方法，被这个给坑了：
> 当你用`functools.lru_cache`装饰一个实例方法，会导致实例方法的内存泄漏，直到这个进程结束。

看下面这个例子：
```python
# src.py
import functools
import time
from typing import TypeVar

Number = TypeVar("Number", int, float, complex)


class SlowAdder:
    def __init__(self, delay: int = 1) -> None:
        self.delay = delay

    @functools.lru_cache
    def calculate(self, *args: Number) -> Number:
        time.sleep(self.delay)
        return sum(args)

    def __del__(self) -> None:
        print("Deleting instance ...")


# Create a SlowAdder instance.
slow_adder = SlowAdder(2)

# Measure performance.
start_time = time.perf_counter()
# ----------------------------------------------
result = slow_adder.calculate(1, 2)
# ----------------------------------------------
end_time = time.perf_counter()
print(f"Calculation took {end_time-start_time} seconds, result: {result}.")


start_time = time.perf_counter()
# ----------------------------------------------
result = slow_adder.calculate(1, 2)
# ----------------------------------------------
end_time = time.perf_counter()
print(f"Calculation took {end_time-start_time} seconds, result: {result}.")
```
这里我创建了一个`SlowAdder`函数，接受一个参数`delay`，它会等待`delay`秒之后计算加法。为了防止方法重复计算导致变慢，用`lru_cache`方法装饰它。`__del__`方法可以告诉我们这个实例什么时候被回收。

这段代码运行结果如下：
```
Copy
Calculation took 2.0021052900010545 seconds, result: 3.
Calculation took 5.632002284983173e-06 seconds, result: 3.
Deleting instance ...
```
可以看到`lru_cache`正常工作，第二次用同样参数调用`calculate`方法相比第一次时间减少了。在第二次运行时，lru装饰器直接在缓存字典里查找了返回值。但`ShowAdder`实例使用没有被内存回收。下一节来证明。

### 垃圾回收无法清除被装饰的实例
如果你用`-i`执行上面的代码，可以直观的看到没有垃圾回收gc运行过。
```python
$ python -i src.py
Calculation took 2.002104839997628 seconds, result: 3.
Calculation took 5.566998879658058e-06 seconds, result: 3.
>>> import gc
>>>
>>> slow_adder.calculate(1,2)
3
>>> slow_adder = None
>>>
>>> gc.collect()
0
>>>
```
在上面的交互界面，我把`slow_adder`指向`None`，然后直接调用垃圾回收，但是并没看到`__del__`方法打印出来的信息。`gc.collect`回显是0，表明有指向`slow_adder`的其他引用存在，导致实例没有被垃圾回收。看看这个引用在哪

```python
$ python -i src.py
Calculation took 2.00233274600032 seconds, result: 3.
Calculation took 5.453999619930983e-06 seconds, result: 3.
>>> slow_adder.calculate.cache_info()
CacheInfo(hits=1, misses=1, maxsize=128, currsize=1)
>>> slow_adder.calculate(1,2)
3
>>> slow_adder.calculate.cache_info()
CacheInfo(hits=2, misses=1, maxsize=128, currsize=1)
>>> slow_adder.calculate.cache_clear()
>>> slow_adder = None
Deleting instance ...
>>>
```

`cache_info()`会展示当前缓存里面的实例。当我手动清除缓存并且将`slow_adder`指向`None`，这时垃圾回收才工作。默认情况下缓存大小是128，但如果我手动设置成`lru_cache(maxsize=None)`，会导致垃圾回收在程序全生命里都无法进行。

这种情况在成大量实例时非常危险。会导致内存泄漏和进程崩溃。

### 解决办法

我们可以把lru装饰器声明为局部变量，随着实例被回收。
```python
# src_2.py
import functools
import time
from typing import TypeVar

Number = TypeVar("Number", int, float, complex)


class SlowAdder:
    def __init__(self, delay: int = 1) -> None:
        self.delay = delay
        self.calculate = functools.lru_cache()(self._calculate)

    def _calculate(self, *args: Number) -> Number:
        time.sleep(self.delay)
        return sum(args)

    def __del__(self) -> None:
        print("Deleting instance ...")
```
和之前唯一的区别就是通过`_calculate`调用之前的计算函数。

```python
$ python -i src.py
>>> slow_adder = SlowAdder(2)
>>> slow_adder.calculate(1,2)
3
>>> slow_adder.calculate.cache_info()
CacheInfo(hits=0, misses=1, maxsize=128, currsize=1)
>>> import gc
>>> slow_adder = None
>>> gc.collect()
Deleting instance ...
11
```
注意这次手动触发垃圾回收(gc.collect())不是必须的。区别于交互窗口的行为，Python解释器会在后台做好垃圾回收。

### 能命中吗
尽管采用了上面的方法，还是有奇怪的事情：
```python
$ python -i src_2.py
>>> slow_adder = SlowAdder(2)
>>> slow_adder.calculate(1,2)
>>> slow_adder
<__main__.SlowAdder object at 0x7f92595f9b40>
>>> slow_adder_2 = SlowAdder(2)
>>> slow_adder_2.calculate(1,2)
3
>>> slow_adder.calculate.cache_info()
CacheInfo(hits=1, misses=2, maxsize=128, currsize=2)
>>> slow_adder_2.calculate.cache_info()
CacheInfo(hits=1, misses=2, maxsize=128, currsize=2)
>>>
```

这里我实例化第一个`slow_adder`并计算之后，第二个实例`slow_adder_2`无法命中第一个的缓存。

这是因为`lru_cache`装饰器用字典来存储缓存数据。所有被装饰函数的输入会被hash成字典的key，字典的value就是对应的返回值。这表示函数的第一个参数`self`也被用来生成key，但是不同实例的self是不同的，这就导致即使其他参数一样，缓存也无法命中。

### 类方法和静态方法呢？
类方法和静态方法没有上诉的困境，因为他们都不绑定到实例。在这些用法中，缓存是对类来说的，而不是实例。先看看类方法的实现：
```python
# src_3.py
import functools
import time


class Foo:
    @classmethod
    @functools.lru_cache
    def bar(cls, delay: int) -> int:
        # Do something with the cls.
        cls.delay = delay
        time.sleep(delay)
        return 42

    def __del__(self) -> None:
        print("Deleting instance ...")


foo_1 = Foo()
foo_2 = Foo()


start_time = time.perf_counter()
# ----------------------------------------------
result = foo_1.bar(2)
# ----------------------------------------------
end_time = time.perf_counter()
print(f"Calculation took {end_time - start_time} seconds, result: {result}.")


start_time = time.perf_counter()
# ----------------------------------------------
result = foo_2.bar(2)
# ----------------------------------------------
end_time = time.perf_counter()
print(f"Calculation took {end_time - start_time} seconds, result: {result}.")
```
可以这么检查垃圾回收行为
```shell
$ python src_3.py
Calculation took 2.0022965140015003 seconds, result: 42.
Calculation took 4.4819971662946045e-06 seconds, result: 42.
>>> foo_1 = None
Deleting instance ...
>>>
```
对于静态方法也是一样，可以像下面这么用：
```python
import functools
import time


class Foo:
    @staticmethod
    @functools.lru_cache
    def bar(delay: int) -> int:
        return 42

    def __del__(self) -> None:
        print("Deleting instance ...")
```

原文参考：
* [functools.lru_cache - Python Docs](https://docs.python.org/3/library/functools.html#functools.lru_cache)
* [Don't lru_cache methods! (intermediate) anthony explains #382](https://www.youtube.com/watch?v=sVjtp6tGo0g)
* [Python LRU cache in a class disregards maxsize limit when decorated with a staticmethod or classmethod decorator](https://stackoverflow.com/questions/70409673/python-lru-cache-in-a-class-disregards-maxsize-limit-when-decorated-with-a-stati)

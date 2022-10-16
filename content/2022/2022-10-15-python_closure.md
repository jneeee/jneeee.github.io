---
date: 2022-10-15
title: Python 的闭包
author: Jn
categories: 工作
tags: 
- Python
---


### 1 前置知识点：
- `for i in range(100)` 循环结束够会留下一个变量 i = 99
- 自由变量 free vars：non-local
- 如果函数中有对变量赋值的操作，并且没有声明全局变量，那么解释器会默认它是局部变量。
例如：
```python
# part1
i = 1
def func1():
    print(i)
func1()
# part2
def func2():
    # global i
    print(i)
    i = 2 # 因为有赋值，所以 i 在 func2 中被认为是 local var
func2()
# UnboundLocalError: local variable 'i' referenced before assignment
```
- func.__code__ 是编译后的函数定义体。`func.__code__.co_freevars` 自由变量（non-local，可以是global和 non-global - local）

### 2 闭包
闭包：在一个外函数中定义了一个内函数，内函数里运用了外函数的临时变量，并且外函数的返回值是内函数的引用。

以下解释源自《流畅的Python》
- 闭包指延伸了作用域的函数，其中包含函数定义体中引用了、但不是在定义体中的非全局变量。函数是不是匿名的没关系，关键是他能访问定义体之外定义的非全局变量。
- 闭包是一种函数，它会保留定义函数时存在的自由变量的绑定，这样调用函数时，虽然定义作用域不可用了，但是仍能使用那些绑定。
注意，只有嵌套在其他函数中的函数才可能需要处理不在全局作用域中的外部变量。

例子
```python
def make_averager():
    queue = []

    def averager(num):
        # averager.__code__.co_freevars: ('i', 'averager')
        queue.append(num) # 引用了外部非全局变量，queue 里的值可以一直存续
        return sum(queue) / len(queue)

    return averager # 外部函数返回内部函数
```

### 3 闭包中的延迟绑定(后期绑定？)
> Python’s closures are late binding. This means that the values of variables used in closures are looked up at the time the inner function is called.
Python 的闭包是延迟绑定的，闭包中的相对 inner function 的外部变量是**在被调用时被加载**。（LOAD_COLSURE）

ref: https://docs.python-guide.org/writing/gotchas/#late-binding-closures


### 4 装饰器
装饰器的实现基于闭包。

```python
def decorator(func_name):
    def handler(*args, **kw):
        # deal with args/kw/return result
        res = func_name(*args, **kw) # func_name 是 handler 的外部非全局变量
        return res
    return handler # 外层函数的返回值是内层函数的引用
```

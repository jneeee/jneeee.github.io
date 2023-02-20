---
date: 2023-02-20
title: C++学习笔记 - 基础篇
author: Jn
categories: 工作
tags:
  - c++
---

C++学习笔记。

参考 https://www.w3schools.com/cpp

### Hello world
C++ 不同于Python的一些基本知识：
- 引号必须是双引号
- C++ 的缩进不是严格的，只是为了代码易读
- 任何变量赋值前必须先声明类型
- 用 g++ 编译后生成二进制文件可以直接执行

相同点：
- 大小写敏感
- ...

```c
#include <iostream>
#include <vector>
#include <string>

using namespace std;

int main()
{
   vector<string> msg {"Hello", "C++", "World", "from", "VS Code", "and the C++ extension!"};
   for (const string& word : msg)
   {
      cout << word << " ";
   }
   cout << endl;
}
```
## 1 变量
类型有 int double char string bool ...
### 1.1 声明（创建）一个变量
语法
```c++
type variableName = value;
```
示例
```
int myNum = 15;
cout << myNum;
```

### 1.2 声明多个变量
```
int x = 5, y = 6, z = 50;
cout << x + y + z;
```
为多个变量赋同一个值
```
int x, y, z;
x = y = z = 50;
cout << x + y + z;
```

### 1.4 常量
当你不想自己或别人修改一个存在的变量时，使用 `const` 关键词。
```
const int myNum = 15;  // myNum will always be 15
myNum = 10;  // error: assignment of read-only variable 'myNum'
```

## 2 用户输入
`cin` 关键词用于获取用户输入。`cout` 是输出。
例子：
```c++
int x; 
cout << "Type a number: "; // Type a number and press enter
cin >> x; // Get user input from the keyboard
cout << "Your number is: " << x; // Display the input value
```

## 3 数据类型
C++ 中的变量必须指定类型。
```c+=
int myNum = 5;               // Integer (whole number)
float myFloatNum = 5.99;     // Floating point number
double myDoubleNum = 9.98;   // Floating point number
char myLetter = 'D';         // Character
bool myBoolean = true;       // Boolean
string myText = "Hello";     // String
```

## 4 运算符

常规的加减乘除 自加自减。赋值：

```
int x = 10;
x += 5;
```
比较符：

== 等于，!= 不等， 大于小于...
逻辑运算：
与：&&
或：||
非： !


## 5 字符串
要使用字符串，需要：
```
#include <string>
string greeting = "Hello";
```

字符串可以相加，也有对象方法 `append`:
```
string firstName = "John ";
string lastName = "Doe";
string fullName = firstName.append(lastName);
string fullName = firstName + " " + lastName;
cout << fullName; // John Doe
cout << firstName; // John Doe
```

fullName 是一个别名。

`+` 用在数字是相加，用在字符串是串联。和 Python 一样。

字符串的长度 对象方法`length()`，`.size()` 功能完全一样。
```
string txt = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
cout << "The length of the txt string is: " << txt.length();
```

字符串的访问。字符串也可以被`index`：
```
string myString = "Hello";
cout << myString[0];
// Outputs H
```

不同于 Python，字符串也可以被修改（这个空间复杂度待确认）：
```
string myString = "Hello";
myString[0] = 'J';
cout << myString;
// Outputs Jello instead of Hello
```

如果需要在字符里打单、双引号，需要用转义字符`\`
```
string txt = "It\'s alright.";
```
常用转义字符：
```
\\
\'
\"
\t
\n
```

常用的关键词 `cout/string` 都是来自 Omitting Namespace，默认写成`std::cout` ，可以用如下方式省略：
```
using namespace std
```

## 6 数学方法
C++ 有很多内建的数学函数

### 6.1 max and min
```
cout << max(5, 10);
cout << min(5, 10);
```

### 6.2 cmath 模块
其他模块比如 `sqrt/round/log` 可以再 cmath 模块里找到
```
#include <cmath>
```
还有很多其他数学函数：https://www.w3schools.com/cpp/cpp_math.asp

## 7 布尔值

```
bool a = 1;
bool isCodingFun = true;
bool isFishTasty = false;
cout << isCodingFun;  // Outputs 1 (true)
cout << isFishTasty;  // Outputs 0 (false)
```

逻辑符号会生成布尔值，比如
```
cout << (1 > 2); // false
```

## 8 条件和 if 语句

条件就是上面提到的生成布尔值的表达式。C++ 支持如下 if 语句：
- if
- else
- else if
- switch

示例：
```
if (20 > 18) {
  cout << "20 is greater than 18";
}

if (condition) {
  // block of code to be executed if the condition is true
} else {
  // block of code to be executed if the condition is false
}

if (condition1) {
  // block of code to be executed if condition1 is true
} else if (condition2) {
  // block of code to be executed if the condition1 is false and condition2 is true
} else {
  // block of code to be executed if the condition1 is false and condition2 is false
}
```

写在一行的条件语句:
```
variable = (condition) ? expressionTrue : expressionFalse;
```

Python:
```Python
a = 1 if True else 2
```

## 9 Switch

用 Switch 关键字从多个block中选择一个执行:

```
switch(expression) {
  case x:
    // code block
    break;
  case y:
    // code block
    break;
  default:
    // code block
}
```

break 关键字会跳过剩余的 case 判断。

default 关键字在没有 case 匹配时执行。

## 10 while 循环

循环是在满足条件时一直执行的代码块。可以节约时间，减少错误，让代码更具可读性。
```
int i = 0;
while (i < 5) {
  cout << i << "\n";
  i++;
}
```

do...while 循环会在检查条件之前，先执行一次 block 里的代码。
```
int i = 0;
do {
  cout << i << "\n";
  i++;
}
while (i < 5);
```
(注：它不支持 break 语句吗？)


## 11 for 循环
当你知道你要执行多少次的时候，使用 for 循环。（注：由此认为 for 是 while 循环的特殊情况）

```
for (statement 1; statement 2; statement 3) {
  // code block to be executed
}
```
- Statement 1 is **executed (one time) before** the execution of the code block.
- Statement 2 defines the condition for executing the code block.
- Statement 3 is **executed (every time) after** the code block has been executed.

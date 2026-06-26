---
title: C++学习指南
date: 2026-04-16 05:34:57
tags: c++学习
---

### C++基础篇

#### C/C++中的sizeof关键字
- <span style="color:green">指针的大小永远是固定的，取决于处理器位数，32位就是 4 字节，64位就是 8 字节</span>
- <span style="color:green">数组作为函数参数时会退化为指针，大小要按指针的计算</span>， 详细请看这篇文章数组退化为指针
- <span style="color:green">struct 结构体要考虑字节对齐: </span>，字节对齐的具体规则请看这篇文章
- <span style="color:green">字节对齐字符串数组要算上末尾的 '\0'</span>

```cpp
// 前提: 64 位电脑上
char  str[] = "Hello World" ;
char   *p = str;
double *dp;
int n = 10;

sizeof(str )＝___12_____  // 11个字符 + 末尾'\0'，总结第四点
sizeof ( p ) =  ___8 ___  // 64 位电脑，指针 8 个字节
sizeof ( n ) = ___4______  // int 一般 4 个字节

void Func (char str[10])
{
   sizeof( str ) = _8__  // 数组做参数退化为 char类型指针，即 8 个字节，总结第2点
}
void *vp = malloc( 100 );

sizeof ( vp )＝__8____  // vp 是一个 void 类型指针，还是 8 个字节

struct AlignedStruct {
    char a;   // 本来1字节，padding 3 字节
    int b;    //  4 字节
    short c;  // 本来 short 2字节，但是整体需要按照 4 字节对齐(成员对齐边界最大的是int 4) ，
    //所以需要padding 2，总共: 4 + 4 + 4
};
```
------
#### C/C++ sizeof 和 strlen 的区别

strlen 是头文件 cstring 中的函数，而 sizeof 是 C++ 中的运算符。
- strlen: strlen 是一个 C 标准库中的函数，用于计算 C 风格字符串（以空字符 '\0' 结尾的字符数组）的长度，即不包括结尾的空字符的字符个数。

```cpp
#include <iostream>
#include <cstring>

int main() {
    char str[] = "Hello, world!";
    std::cout << "Length of str: " << strlen(str) << std::endl; // 输出字符串 str 的长度
}
```

- sizeof: sizeof 是一个 C++ 编译期间计算的操作符，用于计算数据类型或对象所占用的字节数。


```cpp
#include <iostream>

int main() {
    int a = 42;
    std::cout << "Size of int: " << sizeof(int) << std::endl;    // 输出 int 类型的大小
    std::cout << "Size of a: " << sizeof(a) << std::endl;        // 输出变量 a 的大小
    std::cout << "Size of double: " << sizeof(double) << std::endl; // 输出 double 类型的大小
}
```
------
#### C/C++中数组做参数退化为指针

- sizeof 数组参数
    - C++ 面试中还有一个比较常见的考题，就是会将一个数组做参数，然后在函数内部用 sizeof 去判断这个数组参数的大小，如下:

```cpp
int func(char array[]) {
    printf("sizeof=%d\n", sizeof(array));
    printf("strlen=%d\n", strlen(array));
}

int main() {
    char array[] = "Hello World";
    printf("sizeof=%d\n", sizeof(array));
    printf("strlen=%d\n", strlen(array));
    func(array);
}

比如上述的输出结果分别是多少，对于 64 位机器来说，输出结果为：

sizeof=12
strlen=11
sizeof=8
strlen=11
```

- 数组做参数退化为指针
    - 这里涉及到一个概念：数组退化为指针。
    - 数组退化：在 C++ 中，数组在作为函数参数时会退化为指向其首元素的指针。
    - 退化的原因是因为数组作为函数参数时，实际传递的是指向数组首元素的指针，不可能逐个拷贝整个数组然后在栈上传递，所以编译器只知道参数是一个指针，而不知道它的长度信息。
    - 但是，当数组直接作为 sizeof 的参数时，它不会退化，因为 sizeof 是编译器在编译期间计算的结果，这个时候编译器是有信息知道数组的大小。
    - 为了在函数中获取数组的长度，需要将数组的长度作为另一个参数传递给函数，或者使用模板实现。

- 数组的引用做参数 举例

```cpp
#include <iostream>
#include <cstring>
template <typename T, std::size_t N>
void printSizeAndLength(const T (&arr)[N]) {
    std::cout << "Size of arr in function: " << sizeof(arr) << std::endl; // 计算数组的大小
    std::cout << "Length of arr: " << strlen(arr) << std::endl; // 计算字符串的长度
}
int main() {
    char str[] = "Hello, world!";
    std::cout << "Size of str in main: " << sizeof(str) << std::endl; // 计算整个字符数组的大小
    printSizeAndLength(str);
}

result：
Size of str in main: 14
Size of arr in function: 14
Length of arr: 13
```

- 这段代码使用了模板函数 printSizeAndLength，它接受一个数组引用作为参数。
- 在函数内部，使用 sizeof 计算数组的大小时，数组不会退化为指针。
- 引用的作用就在于阻止拷贝的发生，通过传递引用，让形参得到和数组名同样的地址。
------
#### C++ const 关键字详解：修饰变量、指针和函数

在 C/C++ 中，const 是一个关键字，用于表示常量。

const 可以用于修饰变量、函数、指针等，主要作用有以下几种：
1. const 修饰变量
    - 当 const 修饰变量时，该变量将被视为只读变量，即不能被修改。

    - 对于确定不会被修改的变量，应该加上 const，这样可以保证变量的值不会被无意中修改，也可以使编译器在代码优化时更加智能。
    ```cpp
    const int a = 10;
    a = 20; // 编译错误，a 是只读变量，不能被修改
    ```

    但是！！！请注意！！！<br>

    这里的变量只读，其实只是编译器层面的保证，实际上可以通过指针在运行时去间接修改这个变量的值，当然这个方法比较trick。<br>

    对 const int 类型取指针，就是  const int* 类型的指针，将其强制转换为 int* 类型，就去掉了 const 限制，从而修改变量的值。<br>

    在 C++ 中，将 const 类型的指针强制转换为非 const 类型的指针被称为类型强制转换（Type Casting），这种行为称为 const_cast。<br>

    关于 const_cast 可以看下这篇文章: C++几种类型转换的区别<br>

    虽然可以这样操作，但这违反了 const 的语义，可能会导致程序崩溃或者产生未定义行为(undefined behavior)。<br>

    因为编译器可能会做一些优化！！也就是在你用到 const 变量的地方，编译器可能生成的代码直接就替换为常量的值，而不是访问一遍常量的指令。<br>

    所以极大可能你虽然修改了值，但是却不起作用！<br>

    下面👇这个例子，展示了使用 const_cast 修改 const 变量的值却不会起作用：
    ```cpp
    const int a = 10;
    const int* p = &a;
    int* q = const_cast<int*>(p);
    *q = 20;  // 通过指针间接修改 const 变量的值
    std::cout << "a = " << a << std::endl;  // 输出 a 的值，结果为 10
    ```
    在上面的例子中，将 p 声明为 const int* 类型，指向只读变量 a 的地址。<br>

    然后使用 const_cast 将 p 强制转换为  int* 类型的指针 q，从而去掉了 const 限制。<br>

    接下来，通过指针 q 间接修改了变量 a 的值。<br>

    但是请注意，即使 a 的值被修改了，但在程序中输出 a 的值仍然是 10，<br>

    正如前面所有，因为 a 是只读变量，所以编译器做了优化，早就把代码实际替换为了👇下面这样:<br>

    `std::cout << "a = " << 10 << std::endl;`<br>

    总之，使用 const_cast 去掉 const 限制是不推荐的，这会破坏程序的正确性和稳定性。<br>

    我们应该遵循 C/C++ 语言中 const 的语义，尽量不修改只读变量的值。
2. const 修饰函数参数，表示函数不会修改参数<br>
当 const 修饰函数参数时，表示函数内部不会修改该参数的值。这样做可以使代码更加安全，避免在函数内部无意中修改传入的参数值。
```cpp
void func(const int a) {
    // 编译错误，不能修改 a 的值
    a = 10;
}
```

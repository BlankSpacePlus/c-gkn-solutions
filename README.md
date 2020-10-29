# 高克宁著《程序设计基础(C语言)》(第3版)课后题题解

![](/教材封面.jpg)

## 题解章节目录
- [Chapter01 计算机及程序设计概述](/Chapter01.md)
- [Chapter02 信息编码与数据类型](/Chapter02.md)
- [Chapter03 基本运算与顺序结构](/Chapter03.md)
- [Chapter04 逻辑判断与选择结构](/Chapter04.md)
- [Chapter05 迭代计算与循环结构](/Chapter05.md)
- [Chapter06 集合数据与数组](/Chapter06.md)
- [Chapter07 模块化与函数](/Chapter07.md)
- [Chapter08 地址操作与指针](/Chapter08.md)
- [Chapter09 复杂数据类型与结构体](/Chapter09.md)
- [Chapter10 泛化编程与预编译](/Chapter10.md)
- [Chapter11 数据存储与文件](/Chapter11.md)
- [Chapter12 程序设计思想及范例](/Chapter12.md)

## 说明

本人用此书时非初学者，愚以为初学者为应付考试，大可不必深究课后问答题之甚解，只需刷好C语言考研题、C语言计算机二级即可。

切记：需重视大题、需重视细节(无需执着于问答题)。

## 札记
1.`a = a + 7 = b + c`是不对的，`a+7`不是一个变量。

2.C99允许结构体最后一个成员是长度未知的数组，这被称为动态数组成员，结构体的动态数组成员之前至少要包含一个其他成员。<br/>
结构体的大小不包含动态数组成员的大小，计算sizeof()也可以看出这一点。<br/>
可以使用malloc()为含有动态数组成员的结构体变量动态地分配内存空间，所分配的内存空间一般应大于结构体的大小。

3.对匿名结构体成员的引用与普通成员相同。

4.位运算举例：
- 判断偶数：`x&1==0`
- (已知偶数)数值+1：`x=x|1`
- a的低字节+b的高字节得到c：`(a&0x00ff)<<8|(a&0xff00)>>8`
- 交换两个整数：`a=a^b; b=a^b; a=a^b;`

5.三种方式不使用分号输出HelloWorld：
```c
#include <stdio.h>

int main() {
    if(printf("HelloWorld")){}
    return 0;
}
```
```c
#include <stdio.h>

int main() {
    switch(printf("HelloWorld")){}
    return 0;
}
```
```c
#include <stdio.h>

int main() {
    while(!printf("HelloWorld")){}
    return 0;
}
```

6.不直接定义main函数的可执行C源码：
```c
#include<stdio.h>

#define start main

int start() {
    printf("HelloWorld");
    return 0;
}
```

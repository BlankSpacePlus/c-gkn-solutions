# 第三章 基本运算与顺序结构

1.++arg和arg++都能实现自增，直接将++arg赋值给其他变量得到的是arg+1，而将arg++赋值给其他变量得到的是arg(要记得这两个过程中arg自身还是自增1的)。具体细节与编译器有关，不好直接说，不建议深究(遇到那些脑残的++题就当我没说，毫无营养)。<br/>
最简单但不正确的理解是：++arg先执行自增再赋值而arg++先执行赋值再自增。

2.解析如下：
1. int类型，值为24。原因：'\10'是八进制表示，所以其int值为8，ch*3结果为24。
2. float类型，值为0.387500。原因：char自动转型为float，相当于3.1/8.0。
3. double类型，值为8.900001(有浮点误差)。原因：自动转型为double，运算存在浮点数误差，精确值是8.9，默认保留6位小数是8.900001。另外，如果x不是float类型而是double类型，则结果会是8.9。
4. long int类型，值为52。
5. double类型，值为1.995000。
6. int类型，值为3。原因：浮点数强转整数，这是一个截断取整的过程，所以截断小数部分得到3。
7. char类型，值为'a'。注意：如果按照int来输出，则输出97；另外，如果ch='a'这个条件是if条件的布尔表达式，则得到是1(TRUE)。

3. 首先考虑到加减运算的较低优先级，将整个表达式分成三部分：`(float)(a+b)/2`、`(int)x%(int)y`、`x/a`。<br/>
第一个子表达式得到7.0/2=3.5，第二个子表达式得到7%2=1，第三个子表达式得到7.5/2=3.75，所以结果是8.25。

4.所谓的`n%=n+=n-=n*n`，其实就是`n%=(n+=(n-=(n*n)))`，`n*n`有最高的优先级，但不管怎么算`n*n`是没有赋值给任何变量的，所以仍然是`n-=n`得到0(n=0)，然后`n+=0`也是得到0，最后`n%=0`得到0

5.答案是0。理由：&&优先级和()优先级都高于=，所以b==a输出0，由于短路与所以输出0（事实上a+b!=20输出1），直接赋值给c。

6.程序如下：
```c
#include <stdio.h>

int main() {
    int var1 = 55555;
    unsigned int var2 = 55555;
    int var3 = 1234;
    unsigned int var4 = 1234;
    double var5 = -1.2345, var6 = 12345.12345, var7 = 1234567.1234567;
    int var8 = 0123, var9 = 0x123;
    char var10 = 'a';
    printf("%-8d\n", var1);
    printf("%8u\n", var2);
    printf("%10.6d", var1);
    printf("%-8d\n", var3);
    printf("%-8u\n", var4);
    printf("%010lf", var5);
    printf("%-14.2f", var6);
    printf("%14.4f\n", var7);
    printf("%o\n", var8);
    printf("%x\n", var9);
    printf("%04c\n", var10);
    return 0;
}
```

7.程序如下：
```c
#include <stdio.h>

int main() {
    int a, b, c, hour, minute, second;
    char ch;
    long int e;
    double f;
    // (1) 12,34,56
    scanf("%d,%d,%d", &a, &b, &c);
    printf("%d,%d,%d\n", a, b, c);
    // (2) 123456789
    scanf("%3d%3d%3d", &a, &b, &c);
    printf("%d,%d,%d\n", a, b, c);
    // (3) 123c
    scanf("%d%c", &a, &ch);
    printf("%d,%c\n", a, ch);
    // (4) 11:11:11
    scanf("%d:%d:%d", &hour, &minute, &second);
    printf("%d,%d,%d\n", hour, minute, second);
    // (5) 赋给了e和f 55555,55555.555555
    scanf("%ld,%lf", &e, &f);
    printf("%ld,%lf\n", e, f);
    // (6) 123,123
    scanf("%o,%x", &a, &b);
    printf("%o,%x\n", a, b);
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>

int main() {
    printf("请输入一个字母：");
    char c;
    c = getchar();
    switch (c) {
        case 'a':
        case 'A':
            printf("没有前置字母\n后置字母为%c", c+1);
            break;
        case 'z':
        case 'Z':
            printf("前置字母为%c\n没有后置字母", c-1);
            break;
        default:
            printf("前置字母为%c\n后置字母为%c", c-1, c+1);
            break;
    }
    return 0;
}
```

9.代码如下：
```c
#include <stdio.h>
#include <math.h>

int main() {
    printf("请输入圆的半径：");
    double r;
    scanf("%lf", &r);
    printf("圆的周长为：%.2lf\n圆的面积为%.2lf\n", 2*M_PI*r, M_PI*r*r);
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>
#include <math.h>

int main() {
    double x = 0.5;
    printf("%.2lf", sin(x));
    return 0;
}
```

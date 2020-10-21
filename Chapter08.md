# 第八章 地址操作与指针

## 读书笔记

以`*p=&a;`为例。

1.&和*互为逆运算：
- `&*p=&(*p)=&a=p`
- `*&a=*(&a)=*p=a`

2.++/--运算：
- `(*p)++` => `a++`
- `p++` => `p指向a的下一个地址`
- `c = *p++` => `c = *p; *p++`
- `d = *--p` => `d=*--p` => `--p; d = *p;`

3.void指针是一种特殊的指针，void指针无需类型转换即可指向任意类型指针，任何类型的指针也都可以指向void指针，但需要强制类型转换。

4.指针p++合理；数组a++不合理，只能用索引。

5.字符数组与字符指针的区别：
- 存储方式不同。定义字符数组后，系统为其分配一段连续的存储单元；而定义字符型指针变量后，系统只为其分配一个用于存放地址的存储区域。
- 运算方式不同。虽然s和p都代表字符串的首地址，但是s是数组名，相当于一个指针常量，而p是一个指针变量。p++合理，s++不合理。
- 赋值方式不同。s可以进行初始化，不能使用赋值语句进行整体赋值。

6.实参形参的搭配(数组/指针)：
- 实参数组 + 形参数组
- 实参数组 + 形参指针
- 实参指针 + 形参数组
- 实参指针 + 形参指针

7.当主调函数向被调函数传递大量数据时，值传递会将这些数据复制一份给被调函数的形参变量，复制过程占用系统时间。将这些数据组织为数组(或结构体)形式，只需传递一个起始地址给被调函数的形参指针，可以提高执行效率。

8.函数指针多用于指向不同的函数，从而利用指针变量调用这些函数。在多个函数功能各不相同，但返回值和形参列表的个数和数据类型相同的情况下，可以构造一个通用函数，将函数指针作为函数参数，通过函数指针实现多种函数的调用，有利于程序的模块化设计。

9.volatile用于告知编译器，该变量除可被自身程序改变以外，还可被其他程序改变。

10.restrict用于告知编译器，该变量已经被指针所引用，不能通过除该指针外所有其他直接或间接的方式修改该对象的内容。

11.C程序运行时，OS会按照要求为程序中的变量分配内存。可用来存储程序变量的内存有三种：
- 全局(静态)存储区：存放全局变量和静态变量。程序运行结束时该区变量自动释放，未初始化的全局变量和静态变量默认初始值为0。
- 栈：存放函数调用中的各种参数、局部变量、返回值以及函数返回地址。栈由编译器进行管理，自动分配和释放，初始大小与编译器有关。
- 堆：用于程序动态申请和释放空间。

12.函数表达式同样可以是一个指针类型，表示可以通过return语句返回一个地址值，该地址值是指向类型变量的地址。

## 例题训练
【例1】
```c
#include <stdio.h>

int main() {
    int *p, m;
    p = &m;
    scanf("%d", p);
    printf("%d, %d\n", *p, m);
    *p = 15;
    printf("%d, %d\n", *p, m);
    return 0;
}
```

【例2】
```c
#include <stdio.h>

int main() {
    int *p, a[10] = {21, 32, 3, 14, 5, 25, 39, 51, 8, 21};
    int count = 0;
    for (p = a; count < 10; p++, count++) {
        if (*p < 5) {
            printf("%d\t", *p);
        }
    }
    return 0;
}
```

【例3】
```c
#include <stdio.h>

int main() {
    int a[20], i, nLen, nEvenCount = 0, nOddCount = 0, *p = a;
    printf("请输入整数的个数：");
    scanf("%d", &nLen);
    printf("请输入%d个整数：", nLen);
    for (i = 0; i < nLen; i++) {
        scanf("%d", p+i);
    }
    for (i = 0; i < nLen; i++, p++) {
        if (*p % 2 == 0) {
            nEvenCount++;
        } else {
            nOddCount++;
        }
    }
    printf("这组数据中包含%d个偶数，%d个奇数\n", nEvenCount, nOddCount);
    return 0;
}
```

【例4】
```c
#include <stdio.h>

int main() {
    char s1[40], s2[20], *p1, *p2;
    p1 = s1;
    p2 = s2;
    printf("请输入字符串1：");
    gets(s1);
    printf("请输入字符串2：");
    gets(s2);
    while (*p1) {
        p1++;
    }
    while (*p1++=*p2++);
    printf("连接后结果：%s\n", s1);
    return 0;
}
```

【例5】
```c
#include <stdio.h>
#include <string.h>

int main() {
    char str[100], *p;
    int i, nCount = 0;
    p = str;
    printf("请输入英文语句：\n");
    gets(str);
    while (*p != '\0') {
        if (*p == ' ') {
            p++;
            continue;
        } else {
            nCount++;
            i = 0;
            while (*(p+i) != ' ' && *(p+i) != '\0') {
                i++;
            }
            p += i;
        }
    }
    printf("该句子包含%d个单词\n", nCount);
    return 0;
}
```

【例6】(说明：swap()返回值应该改成void而不是int)
```c
#include <stdio.h>

void swap(int *p1, int *p2) {
    int temp;
    temp = *p1;
    *p1 = *p2;
    *p2 = temp;
}

int main() {
    int a = 5, b = 9, *pa = &a, *pb = &b;
    printf("交换前：a=%d, b=%d\n", a, b);
    swap(pa, pb);
    printf("交换后：a=%d, b=%d\n", a, b);
    return 0;
}
```

【例7】(#include没写<stdio.h>)
```c
#include <stdio.h>
#include <math.h>

void process(float f1, float f2, float f3, float *p1, float *p2) {
    float s;
    s = (f1 + f2 + f3) / 2;
    *p1 = sqrt(s*(s-f1)*(s-f2)*(s-f3));
    *p2 = f1 + f2 +f3;
}

int main() {
    float a, b, c, fArea, fPerimeter;
    printf("请输入三角形的三边：");
    scanf("%f %f %f", &a, &b, &c);
    process(a, b, c, &fArea, &fPerimeter);
    printf("Area=%f, Perimeter=%f", fArea, fPerimeter);
    return 0;
}
```

【例8】
```c
#include <stdio.h>

void inv1(int b[], int n) {
    int temp, i, j, m=(n-1)/2;
    for (i = 0; i <= m; i++) {
        j = n - 1 - i;
        temp = b[i];
        b[i] = b[j];
        b[j] = temp;
    }
}

int main() {
    int i, a[6] = {1, 3, 4, 6, 7, 9};
    printf("折半交换前：");
    for (i = 0; i < 6; i++) {
        printf("%3d", a[i]);
    }
    inv1(a, 6);
    printf("\n折半交换后：");
    for (i = 0; i < 6; i++) {
        printf("%3d", a[i]);
    }
    printf("\n");
    return 0;
}
```

【例9】
```c
#include <stdio.h>

int myStrLen(char *s);

void myStrCpy(char *s1, char *s2);

void myStrUpr(char *p);

int main() {
    char s1[50], s[50];
    int length;
    printf("请输入一个字符串：");
    gets(s1);
    length = myStrLen(s1);
    myStrCpy(s1, s);
    myStrUpr(s);
    printf("字符串长度：%d\n", length);
    printf("字符串复制并大写前后分别是：%s, %s\n", s1, s);
    return 0;
}

int myStrLen(char *s) {
    int i = 0;
    while (*s != '\0') {
        i++;
        s++;
    }
    return i;
}

void myStrCpy(char *s1, char *s2) {
    while (*s1 != '\0') {
        *s2 = *s1;
        s1++;
        s2++;
    }
    *s2 = '\0';
}

void myStrUpr(char *p) {
    while (*p != 0) {
        if (*p >= 'a' && *p <= 'z') {
            *p -= 32;
        }
        p++;
    }
}
```

【例10】
```c
#include <stdio.h>
#include <string.h>

#define SIZE 100

char buf[SIZE];

char *p = buf;

char *alloc(int n) {
    char *begin;
    if (p + n <= buf + SIZE) {
        begin = p;
        p += n;
        return begin;
    }
    return NULL;
}

int main() {
    char *p1, *p2;
    int i;
    p1 = alloc(4);
    strcpy(p1, "123");
    p2 = alloc(5);
    strcpy(p2, "abcd");
    printf("buf = %p\n", buf);
    printf("p1 = %p\n", p1);
    printf("p2 = %p\n", p2);
    puts(p1);
    puts(p2);
    for (i = 0; i < 9; i++) {
        printf("%c", buf[i]);
    }
    return 0;
}
```

【例11】
```c
#include <stdio.h>
#include <math.h>

float f1 (float x) {
    return x * x + 2 * x +1;
}

float f2 (float x) {
    return 2 * sin(x);
}

float f3 (float x) {
    return 2 * x +1;
}

/**
 * @param p *p为指向函数的指针
 * @param fPos1 左区间的值
 * @param fPos2 右区间的值
 * @return
 */
float getMin(float (*p)(float), float fPos1, float fPos2) {
    float f, t, fMin, fStep=0.01;
    fMin = (*p)(fPos1);
    for (f = fPos1; f <= fPos2; f+=fStep) {
        t = (*p)(f);
        if (t < fMin) {
            fMin = t;
        }
    }
    return fMin;
}

int main() {
    printf("f1函数最小值：%f\n", getMin(f1, -1, 1));
    printf("f2函数最小值：%f\n", getMin(f2, 1, 3));
    printf("f3函数最小值：%f\n", getMin(f3, -1, 1));
    return 0;
}
```

【例12】
```c
#include <stdio.h>

int main() {
    char *pSeason[4] = {"Spring", "Summer", "Autumn", "Winter"};
    int month;
    printf("请输入月份(1~12)：");
    scanf("%d", &month);
    switch (month) {
        case 12:
        case 1:
        case 2:
            printf("%s", pSeason[0]);
            break;
        case 3:
        case 4:
        case 5:
            printf("%s", pSeason[1]);
            break;
        case 6:
        case 7:
        case 8:
            printf("%s", pSeason[2]);
            break;
        case 9:
        case 10:
        case 11:
            printf("%s", pSeason[3]);
            break;
        default:
            printf("输入错误！");
    }
    return 0;
}
```

【例13】
```c
#include <stdio.h>
#include <string.h>

void sortString(int n, char * str[]) {
    char *c;
    int i, j;
    for (i = 0; i <= n-2; i++) {
        for (j = 0; j <= n-2-i; j++) {
            if (strcmp(str[j], str[j+1]) > 0) {
                c = str[j];
                str[j] = str[j+1];
                str[j+1] = c;
            }
        }
    }
}

int main() {
    int i;
    char * lan[] = {"China", "France", "Arab"};
    sortString(3, lan);
    for (i = 0; i < 3; i++) {
        printf("%s\n", lan[i]);
    }
    return 0;
}
```

【例14】
```c
#include <stdio.h>

int main(int argc, char * argv[]) {
    int i;
    printf("所有的命令行参数：\n");
    for (i = 0; i < argc; i++) {
        printf("\n参数%d = %s", i+1, argv[i]);
    }
    return 0;
}
```

【例15】
```c
#include <stdio.h>

int main() {
    int a[3][4] = {{11, 21, 33, 42}, {15, 22, 32, 13}, {41, 32, 24, 16}};
    int (*p)[4], i, j;
    p = a;
    for (i = 0; i < 3; i++) {
        for (j = 0; j <4; j++) {
            printf("%2d ", *(*(p+i)+j));
        }
        printf("\n");
    }
    return 0;
}
```

【例16】
```c
#include <stdio.h>

int main() {
    int i;
    char *pArray[] = {"One", "Two", "Three"};
    char **p;
    p = pArray;
    for (i = 0; i < 3; i++, p++) {
        printf("%s\n", *p);
    }
    return 0;
}
```

## 课后习题
1.解析：
```c
#include <stdio.h>

int f1 (int p) {
    return p++;
}

int f2 (int *p) {
    return *(p++);
}

int f3 (int *p) {
    return (*p)++;
}

int *f4 (int *p) {
    return p++;
}

int main() {
    int n = 4;
    int *p = &n;
    printf("%d\n", f1(n)); // 4
    printf("%d\n", n); // 4
    printf("%d\n", f2(p)); // 4
    printf("%d\n", n); // 4
    printf("%d\n", f3(p)); // 4
    printf("%d\n", n); // 5
    printf("%d\n", *f4(p)); // 5
    printf("%d\n", n); // 5
    return 0;
}
```
说明如下：
- (1) 值传递，改变的作用域在函数内部
- (2) 地址++但不会返回，返回的是原地址，原值也不变
- (3) 引用传递，值++且会直接改到地址里
- (4) 指针++然后返回指针其实没有作用

2.代码如下：
```c
#include <stdio.h>

int main() {
    int a = 1, b = 2;
    int *pa = &a, *pb = &b;
    printf("%d, %d\n", *pa, *pb);
    printf("%d + %d = %d\n", *pa, *pb, *pa + *pb);
    printf("%d - %d = %d\n", *pa, *pb, *pa - *pb);
    printf("%d * %d = %d\n", *pa, *pb, *pa * *pb);
    printf("%d / %d = %d\n", *pa, *pb, *pa / *pb);
    return 0;
}
```

3.代码如下：
```c
#include <stdio.h>

void multiplyArray(int *a, int m) {
    for (int i = 0; i < 10; i++) {
        *(a+i) *= m;
    }
}

int main() {
    int a[10] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    multiplyArray(a, 5);
    for (int i = 0; i < 10; i++) {
        printf("%d ", a[i]);
    }
    return 0;
}
```

4.代码如下：
```c
#include <stdio.h>

void getData(int *a, int num);

void reverse(int *a, int num);

void showData(int *a, int num);

int main() {
    int a[10];
    getData(a, 10);
    reverse(a, 10);
    showData(a, 10);
    return 0;
}

void getData(int *a, int num) {
    for (int i = 0; i < num; i++) {
        scanf("%d", a+i);
    }
}

void reverse(int *a, int num) {
    for (int i = 0; i < num/2; i++) {
        int temp = *(a+i);
        *(a+i) = *(a+9-i);
        *(a+9-i) = temp;
    }
}

void showData(int *a, int num) {
    for (int i = 0; i < num; i++) {
        printf("%d ", *(a+i));
    }
}
```

5.代码如下：
```c
#include <stdio.h>

int max(int a[], int n, int *p) {
    int max = a[0], i;
    for (i = 0; i < n; i++) {
        if (a[i] > max) {
            max = a[i];
            *p = i;
        }
    }
    return max;
}

int min(int a[], int n, int *p) {
    int min = a[0], i;
    for (i = 0; i < n; i++) {
        if (a[i] < min) {
            min = a[i];
            *p = i;
        }
    }
    return min;
}

int main() {
    int a[10] = {1, 22, 3, 4, 155, -6, 7, 8888, 9, 1}, index = 0;
    int *p = &index;
    int max_num = max(a, 10, p);
    printf("最大值的值是：%d，位置是：%d\n", max_num, *p);
    int min_num = min(a, 10, p);
    printf("最小值的值是：%d，位置是：%d\n", min_num, *p);
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>

void myitoa(int n, char *str) {
    int i = 0;
    while (n != 0) {
        str[i] = (char)(n / 10 + '0');
        n /= 10;
        i++;
    }
}

int main() {
    int n = 10;
    // 考虑到int上限
    char str[15];
    myitoa(n, str);
    printf("%s", str);
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>

void rotateArray(int *a, int m, int n) {
    // 这里是一个细节，要小心n大于m
    n %= m;
    int temp[m];
    for (int i = 0; i < n; i++) {
        temp[i] = a[m-n+i];
    }
    // 一定要倒着来
    for (int i = m-n-1; i >= 0; i--) {
        a[n+i] = a[i];
    }
    for (int i = 0; i < n; i++) {
        a[i] = temp[i];
    }
}

int main() {
    int n = 10;
    int a[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 0};
    for (int i = 0; i < 10; i++) {
        printf("%d ", a[i]);
    }
    rotateArray(a, 10, 3);
    printf("\n");
    for (int i = 0; i < 10; i++) {
        printf("%d ", a[i]);
    }
    return 0;
}
```

8.代码如下(题目要求的int[][]形参是不符合要求的，应该传指针)：
```c
#include <stdio.h>

void fun(int *a, int n, int m, int *odd, int *even) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (*(a+i*m+j) % 2 == 0) {
                (*even)++;
            } else {
                (*odd)++;
            }
        }
    }
}

int main() {
    int n = 4, m = 3, odd = 0, even = 0, *odd_ptr = &odd, *even_ptr = &even;
    int a[4][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}, {10, 11, 15}};
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            printf("%d ", a[i][j]);
        }
    }
    fun(a, n, m, odd_ptr, even_ptr);
    printf("\n偶数个数为%d，奇数个数为%d\n", even, odd);
    return 0;
}
```

9.代码如下：
```c
#include <stdio.h>
#include <string.h>

int strCount(char *str1, char *str2) {
    int counter = 0;
    int len1 = strlen(str1), len2 = strlen(str2), flag = 1, i, j;
    for (i = 0; i <= len1-len2; i++) {
        flag = 1;
        for (j = 0; j < len2; j++) {
            if (str1[i+j] != str2[j]) {
                flag = 0;
                break;
            }
        }
        if (flag) {
            counter++;
        }
    }
    return counter;
}

int main() {
    char str1[] = "howareyouareGGGare", str2[] = "are";
    printf("\"%s\"在\"%s\"内出现的次数是%d", str2, str1, strCount(str1, str2));
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>
#include <string.h>

char *strToS(char *str) {
    int length = strlen(str), newLength = 0, i = 0, j = 0;
    char result[length], firstChar, tempChar, *ptr;
    for (i = 0; i < length; i++) {
        tempChar = str[i];
        if (tempChar == ' ') {
            if (j > 3) {
                result[newLength] = firstChar > 'a' ? firstChar-('a'-'A') : firstChar;
                newLength++;
            }
            j = 0;
        } else {
            if (j == 0) {
                firstChar = tempChar;
            }
            j++;
        }
    }
    if (j > 3) {
        result[newLength] = firstChar > 'a' ? firstChar-('a'-'A') : firstChar;
        newLength++;
    }
    if (newLength == 0) {
        return "null";
    }
    ptr = result;
    return ptr;
}

int main() {
    char str[] = "I love you very much", *result = strToS(str);
    if (strcmp(result, "null") == 0) {
        printf("%s找不到合适的缩写字符串", str);
    } else {
        printf("%s的缩写字符串是：%s", str, result);
    }
    return 0;
}
```

[Chapter09 复杂数据类型与结构体](/Chapter09.md)

[返回首页](/README.md)

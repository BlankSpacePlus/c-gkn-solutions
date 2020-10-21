# 第七章 模块化与函数

1.略

2.代码如下：
```c
#include <stdio.h>
#include <ctype.h>

int isChar(char c) {
    if (isalpha(c) || isdigit(c)) {
        return c;
    }
    return 0;
}

int main() {
    printf("请输入一个字符：");
    char c;
    scanf("%c", &c);
    printf("%d", isChar(c));
    return 0;
}
```

3.代码如下：
```c
#include <stdio.h>

int isPrime(int n) {
    for (int i = 2; i < n; i++) {
        if (n % i == 0) {
            return 0;
        }
    }
    return 1;
}

int main() {
    printf("请输入两个整数：");
    int m, n;
    scanf("%d %d", &m, &n);
    for (int i = m; i <= n; i++) {
        if (isPrime(i)) {
            printf("%d ", i);
        }
    }
    return 0;
}
```

4.代码如下(对题意稍有调整)：
```c
#include <stdio.h>

double fun(int x, int n) {
    double result = 1, temp = 1;
    int num = 1;
    for (int i = 1; i <= n; i++) {
        num *= i;
        temp *= x;
        result += (temp / num);
    }
    return result;
}

int main() {
    printf("请分别输入n和x：");
    int n, x;
    scanf("%d %d", &n, &x);
    printf("%lf", fun(x, n));
    return 0;
}
```

5.代码如下：
```c
#include <stdio.h>

int max_fun(int a, int b) {
    int temp;
    while (b > 0) {
        temp = a % b;
        a = b;
        b = temp;
    }
    return a;
}

int min_fun(int a, int b) {
    return a * b / max_fun(a, b);
}

int main() {
    printf("请输入两个正整数：");
    int a, b;
    scanf("%d %d", &a, &b);
    printf("最大公约数为：%d\n最小公倍数为：%d\n", max_fun(a, b), min_fun(a, b));
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>

void fun(int a[], int n, int x) {
    for (int i = 0; i < n; i++) {
        if (a[i] != x) {
            printf("%d ", a[i]);
        }
    }
}

int main() {
    int a[5] = {1, 2, 3, 4, 5};
    fun(a, 5, 3);
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>

int max_a(int a[], int n) {
    int max = a[0];
    for (int i = 0; i < n; i++) {
        if (a[i] > max) {
            max = a[i];
        }
    }
    return max;
}

int min_a(int a[], int n) {
    int min = a[0];
    for (int i = 0; i < n; i++) {
        if (a[i] < min) {
            min = a[i];
        }
    }
    return min;
}

int ave_a(int a[], int n) {
    int sum = 0;
    for (int i = 0; i < n; i++) {
        sum += a[i];
    }
    return sum / n;
}

int main() {
    int a[10] = {3, 1, -3, 10000, 4, 333, -300, 2, 6, 10};
    printf("最大值为：%d\n最小值为：%d\n平均值为：%d\n", max_a(a, 10), min_a(a, 10), ave_a(a, 10));
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>

// 加密
int encrypt(int num) {
    int a, b, c, d = num;
    a = d / 1000;
    d %= 1000;
    b = d / 100;
    d %= 100;
    c = d / 10;
    d %= 10;
    a = (a+5)%10;
    b = (b+5)%10;
    c = (c+5)%10;
    d = (d+5)%10;
    return d*1000+c*100+b*10+a;
}

int main() {
    int num = 1234;
    int cipher = encrypt(num);
    printf("加密后的密文是：%d\n", cipher);
    // 加密算法和解密算法完全一样
    printf("解密后的明文是：%d\n", encrypt(cipher));
    return 0;
}
```

9.代码如下(没做越界检查)：
```c
#include <stdio.h>

/**
 * 复制子串
 * @param str1
 * @param m 从0开始
 * @param n 复制的长度
 * @param str2
 */
void str_mid(char str1[], int m, int n, char str2[]) {
    for (int i = 0; i < n; i++) {
        str2[i] = str1[m+i];
    }
}

int str_len(char s[]) {
    int counter = 0;
    char c = s[0];
    while (c != '\0') {
        c = s[++counter];
    }
    return counter;
}

int main() {
    char str1[] = "goodmorning", str2[10];
    int m = 1, n = 3;
    str_mid(str1, m, n, str2);
    printf("原字符串为：%s，长度为%d\n", str1, str_len(str1));
    printf("复制后的子串为：%s，长度为%d\n", str2, str_len(str2));
    return 0;
}
```

10.代码如下(非递归)：
```c
#include <stdio.h>

int fib(int n) {
    int prev = 0, next = 1, temp;
    if (n == 0) {
        return prev;
    } else if (n == 1) {
        return next;
    }
    for (int i = 2; i <= n; i++) {
        temp = prev + next;
        prev = next;
        next = temp;
    }
    return next;
}

int main() {
    int n = 10;
    printf("第%d个斐波那契数是%d", n, fib(n));
    return 0;
}
```

[Chapter08 地址操作与指针](/Chapter08.md)

[返回首页](/README.md)

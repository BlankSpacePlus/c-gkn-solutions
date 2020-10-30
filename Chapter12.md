# 第十二章 程序设计思想及范例

## 例题改编

### 求和求积问题

【累加求和】

```c
#include <stdio.h>

int main() {
    int sum = 0, i, n;
    scanf("%d", &n);
    for (i = 1; i <= n; i++) {
        sum += i;
    }
    printf("%d", sum);
    return 0;
}
```

其他思路：使用等差数列求和公式：<br/>
![](http://latex.codecogs.com/gif.latex?Sum=\sum\limits_{i=1}^{n}{i})

【多项式求和】

![](http://latex.codecogs.com/gif.latex?\frac{{\pi}^{2}}{6}=\frac{1}{{1}^{2}}+\frac{1}{{2}^{2}}+\cdots+\frac{1}{{n}^{2}})

```c
#include <stdio.h>
#include <math.h>

int main() {
    double pi = 0;
    double y = 0;
    double an;
    unsigned long n;
    for (n = 1; n < 1000; n++) {
        an = 1.0/(n*n);
        y += an;
    }
    pi = sqrt(6.0*y);
    printf("π=%.6f", pi);
    return 0;
}
```

【斐波那契求和】

斐波那契数列：`1, 1, 2, 3, 5, 8, 13, 21, 34, ...`

```c
#include <stdio.h>

int main() {
    unsigned long long prev = 1, next = 1, sum = 0, temp;
    int n, i;
    scanf("%d", &n);
    if (n == 1) {
        sum = 1;
        printf("%llu", sum);
        return 0;
    }
    sum = 2;
    if (n == 2) {
        printf("%llu", sum);
        return 0;
    }
    for (i = 2; i < n; i++) {
        temp = prev;
        prev = next;
        next += temp;
        sum += next;
    }
    printf("%llu", sum);
    return 0;
}
```

【累乘求阶乘】

![](http://latex.codecogs.com/gif.latex?n!={1}\times{2}\times{3}\times\cdots\times{n})

```c
#include <stdio.h>

int main() {
    unsigned long long result = 1;
    int n;
    scanf("%d", &n);
    for (int i = 1; i <= n; i++) {
        result *= i;
    }
    printf("%llu", result);
    return 0;
}
```

### 遍历问题

【百钱买百鸡】

- 公鸡5铜钱1个
- 母鸡3铜钱1个
- 雏鸡1铜钱3个

```c
#include <stdio.h>

int main() {
    int counter = 0, i, j, k;
    for (i = 0; i <= 20; i++) {
        for (j = 0; j <= 33; j++) {
            for (k = 0; k <= 100; k++) {
                if (i+j+k==100 && i*15+j*9+k==300) {
                    printf("共买%2d只公鸡、%2d只母鸡、%2d只雏鸡\n", i, j, k);
                    counter++;
                }
            }
        }
    }
    printf("一共有%d种购买方案", counter);
    return 0;
}
```

【完数】

完数：一个数除了自身的所有因子之和恰好等于其本身

```c
#include <stdio.h>

int main() {
    int ptr, arr[1000], temp_sum, i, j;
    for (i = 1; i < 1000; i++) {
        temp_sum = 0;
        ptr = 0;
        for (j = 1; j < i; j++) {
            if (i % j == 0) {
                arr[ptr] = j;
                ptr++;
                temp_sum += j;
            }
        }
        if (temp_sum == i) {
            printf("%d是完数，其因子为：", i);
            for (j = 0; j < ptr; j++) {
                printf("%d ", arr[j]);
            }
            printf("\n");
        }
    }
    return 0;
}
```

【水仙花数】

水仙花数：三位数的每一位数值的立方和等于这个数本身

```c
#include <stdio.h>

int main() {
    int i, j, k, temp;
    for (i = 1; i <= 9; i++) {
        for (j = 0; j <= 9; j++) {
            for (k = 0; k <= 9; k++) {
                temp = i*100+j*10+k;
                if (i*i*i+j*j*j+k*k*k == i*100+j*10+k) {
                    printf("%d\n", temp);
                }
            }
        }
    }
    return 0;
}
```

### 迭代问题

【二分迭代法】

求方程 ![](http://latex.codecogs.com/gif.latex?2x^{3}-4x^{2}+3x-6=0) 的实根

```c
#include <stdio.h>
#include <math.h>

double function(double x) {
    return 2*x*x*x - 4*x*x + 3*x - 6;
}

int main() {
    double x1, x2, x0, fx1, fx2, fx0;
    x1 = -10;
    x2 = 10;
    fx1 = function(x1);
    fx2 = function(x2);
    do {
        x0 = (x1+x2)/2.0;
        fx0 = function(x0);
        if (fx0 * fx1 < 0) {
            x2 = x0;
            fx2 = fx0;
        } else {
            x1 = x0;
            fx1 = fx0;
        }
    } while (fabs(fx0) >= 1e-6);
    printf("方程的根为%lf", x0);
    return 0;
}
```

【牛顿迭代法】

求方程 ![](http://latex.codecogs.com/gif.latex?2x^{3}-4x^{2}+3x-6=0) 的实根

牛顿迭代法：![](http://latex.codecogs.com/gif.latex?x_{k+1}=x_{k}-\frac{f(x_{k})}{f'(x_{k})})

```c
#include <stdio.h>
#include <math.h>

double derivative(double x) {
    return 6*x*x - 8*x + 3;
}

double function(double x) {
    return 2*x*x*x - 4*x*x + 3*x - 6;
}

int main() {
    double x, x0, fx, f;
    x = 1.5;
    do {
        x0 = x;
        fx = function(x0);
        f = derivative(x);
        x = x0 - fx/f;
    } while (fabs(x-x0) >= 1e-6);
    printf("方程的根为%lf", x0);
    return 0;
}
```

### 排序问题

【直接插入排序】

```c

```

【冒泡排序】

```c

```

【选择排序】

```c

```

【希尔排序】

```c

```

【归并排序】

```c

```

【堆排序】

```c

```

【快速排序】

```c

```

### 查找问题

【顺序查找】

```c
#include <stdio.h>

int searchBySequence(int data, int s[], int n) {
    int i = -1, j = 0;
    for (j = 0; j < n; j++) {
        if (s[j] == data) {
            i = j;
            break;
        }
    }
    return i;
}

int main() {
    int s[] = {-2, 3, 5, 6, 8, 12, 32, 56, 85, 95, 101};
    int i, data = 6;
    i = searchBySequence(data, s, 11);
    printf("%d", i);
    return 0;
}
```

【二分查找】

```c
#include <stdio.h>

int searchByMid(int data, int s[], int n) {
    int low = 0, mid, high = n-1;
    while (low <= high) {
        mid = (low + high) / 2;
        if (data == s[mid]) {
            return mid;
        } else if (data < s[mid]) {
            high = mid-1;
        } else {
            low = mid + 1;
        }
    }
    return -1;
}

int main() {
    int s[] = {-2, 3, 5, 6, 8, 12, 32, 56, 85, 95, 101};
    int i, data = 6;
    i = searchByMid(data, s, 11);
    printf("%d", i);
    return 0;
}
```

### 递归问题

【汉诺塔】

```c
#include <stdio.h>

void move(char chSour, char chDest) {
    printf("%c -> %c\n", chSour, chDest);
}

void hanoi(int n, char chA, char chB, char chC) {
    if (n == 1) {
        move(chA, chC);
    } else {
        hanoi(n-1, chA, chC, chB);
        move(chA, chC);
        hanoi(n-1, chB, chA, chC);
    }
}

int main() {
    int n;
    printf("请输入盘子的数量：");
    scanf("%d", &n);
    printf("将%d个盘子从A移动到C的过程为：\n", n);
    hanoi(n, 'A', 'B', 'C');
    return 0;
}
```

### 矩阵运算

【矩阵加减法】

```c
#define M 10
#define N 8

void matrixAdd(int a[M][N], int b[M][N], int c[M][N]) {
    int i, j;
    for (i = 0; i < M; i++) {
        for (j = 0; j < N; j++) {
            c[i][j] = a[i][j] + b[i][j];
        }
    }
}

void matrixSubtract(int a[M][N], int b[M][N], int c[M][N]) {
    int i, j;
    for (i = 0; i < M; i++) {
        for (j = 0; j < N; j++) {
            c[i][j] = a[i][j] - b[i][j];
        }
    }
}
```

【矩阵乘法】

```c
#define M 10
#define N 8
#define P 9

void matrixMultiply(int a[M][N], int b[N][P], int c[M][P]) {
    int i, j, k, sum;
    for (i = 0; i < M; i++) {
        for (k = 0; k < P; k++) {
            sum = 0;
            for (j = 0; j < N; j++) {
                sum += a[i][j] * b[j][k];
            }
            c[i][k] = sum;
        }
    }
}
```

【矩阵转置】

```c
#define M 10
#define N 8

void matrixTranspose(int a[M][N], int b[M][N]) {
    int i, j;
    for (i = 0; i < M; i++) {
        for (j = 0; j < N; j++) {
            b[i][j] = a[j][i];
        }
    }
}
```

## 课后习题

1.代码如下：
```c

```

2.代码如下：
```c

```

3.代码如下：
```c

```

4.代码如下：
```c

```

5.代码如下：
```c

```

6.代码如下：
```c

```

7.代码如下：
```c

```

8.代码如下：
```c

```

9.代码如下：
```c

```

10.代码如下：
```c

```

[Chapter13 面向对象与C++基础](/Chapter13.md)

[返回首页](/README.md)

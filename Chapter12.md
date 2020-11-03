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
void insertSort(int a[], int n) {
    int i, j, temp;
    for (i = 1; i < n; ++i) {
        temp = a[i];
        j = i - 1;
        while (j >= 0 && temp < a[j]) {
            a[j+1] = a[j];
            --j;
        }
        a[j+1] = temp;
    }

}
```

【冒泡排序】

```c
void bubbleSort(int a[], int n) {
    int i, j, flag, temp;
    for (i = n-1; i >= 1; --i) {
        flag = 0;
        for (j = 1; j <= i; ++j) {
            if (a[j-1] > a[j]) {
                temp = a[j];
                a[j] = a[j-1];
                a[j-1] = temp;
                flag = 1;
            }
        }
        if (!flag) {
            return;
        }
    }
}
```

【选择排序】

```c
void selectSort(int a[], int n) {
    int i, j, k, temp;
    for (i = 0; i < n; ++i) {
        k = i;
        for (j = i+1; j < n; ++j) {
            if (a[k] > a[j]) {
                k = j;
            }
        }
        temp = a[i];
        a[i] = a[k];
        a[k] = temp;
    }
}
```

【希尔排序】

```c
void shellSort(int a[], int n) {
    int i, j, k;
    for (i = n/2; i >= 1; i /= 2) {
        for (j = i+1; j <= n; ++j) {
            if (a[j] < a[j-i]) {
                a[0] = a[j];
                for (k = j-i; k>0 && a[0]<a[k]; k-=i) {
                    a[k+i] = a[k];
                }
                a[k+i] = a[0];
            }
        }
    }
}
```

【归并排序】

```c
void merge(int a[], int b[], int low, int mid, int high) {
    int i, j, k;
    for (i = low; i <= high; ++i) {
        b[i] = a[i];
    }
    for (i = low, j = mid+1, k = i; i<=mid && j<=high; ++k) {
        if (b[i] <= b[j]) {
            a[k] = b[i++];
        } else {
            a[k] = b[j++];
        }
        while (i <= mid) {
            a[k++] = b[i++];
        }
        while (j <= high) {
            a[k++] = b[j++];
        }
    }
}

void mergeSort(int a[], int low, int high, int n) {
    // 辅助数组
    int *b = (int *)malloc((n+1)*sizeof(int));
    if (low < high) {
        int mid = (low + high) / 2;
        mergeSort(a, low, mid, n);
        mergeSort(a, mid+1, high, n);
        merge(a, b, low, mid, high);
    }
}
```

【堆排序】

```c
void shift(int a[], int low, int high) {
    int i = low, j = 2*i, temp = a[i];
    while (j <= high) {
        if (j < high && a[j] < a[j+1]) {
            ++j;
        }
        if (temp < a[j]) {
            a[i] = a[j];
            i = j;
            j = 2*i;
        } else {
            break;
        }
    }
    a[i] = temp;
}

void heapSort(int a[], int n) {
    int i, temp;
    // 建堆
    for (i = n/2; i >= 1; --i) {
        shift(a, i, n);
    }
    // 堆排序
    for (i = n; i >= 2; --i) {
        temp = a[1];
        a[1] = a[i];
        a[i] = temp;
        shift(a, 1, i-1);
    }
}
```

【快速排序】

```c
void quickSort(int a[], int low, int high) {
    int temp;
    int i = low, j = high;
    if (low < high) {
        temp = a[low];
        while (i < j) {
            while (j > i && a[j] >= temp) {
                --j;
            }
            if (i < j) {
                a[i] = a[j];
                ++i;
            }
            while (i < j && a[i] < temp) {
                ++i;
            }
            if (i < j) {
                a[j] = a[i];
                --j;
            }
        }
        a[i] = temp;
        quickSort(a, low, i-1);
        quickSort(a, i+1, high);
    }
}
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
#include <stdio.h>

int main() {
    int i, n;
    double result = 1, temp = 1;
    scanf("%d", &n);
    for (i = 1; i <= 1000; i++) {
        temp *= n;
        result += 1/temp;
    }
    printf("结果是%.6f", result);
    return 0;
}
```

2.代码如下：
```c
#include <stdio.h>
#include <math.h>

double derivative(double x) {
    return 6*x*x - 8*x + 1;
}

double function(double x) {
    return 2*x*x*x - 4*x*x + x - 2;
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

3.代码如下：
```c
#include <stdio.h>
#include <math.h>

double derivative(double x) {
    return -sin(x) - 1;
}

double function(double x) {
    return cos(x) - x;
}

int main() {
    double x, x0, fx, f;
    x = 0;
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

4.代码如下：
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main() {
    int i, j, n = 1000, flag1 = 1, flag2 = 1, temp_len;
    char str[20];
    for (i = 2; i < n; ++i) {
        flag1 = 1;
        flag2 = 1;
        for (j = 2; j < i; ++j) {
            if (i % j == 0) {
                flag1 = 0;
                break;
            }
        }
        if (flag1) {
            itoa(i, str, 10);
            temp_len = strlen(str);
            for (j = 0; j < temp_len/2; ++j) {
                if (str[j] != str[temp_len-j-1]) {
                    flag2 = 0;
                    break;
                }
            }
            if (flag2) {
                printf("%d是回文素数\n", i);
            }
        }
    }
    return 0;
}
```

5.代码如下：
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int fun(int x) {
    unsigned i = x;
    int a_len, b_len, j;
    char a[1000], b[1000];
    ulltoa(i, a, 10);
    ulltoa(i*i, b, 10);
    a_len = strlen(a);
    b_len = strlen(b);
    for (j = 0; j < a_len; j++) {
        if (a[a_len-1-j] != b[b_len-1-j]) {
            return 0;
        }
    }
    return 1;
}

int main() {
    int i, n;
    scanf("%d", &n);
    for (i = 1; i <= n; i++) {
        if (fun(i)) {
            printf("%d是同构数\n", i);
        }
    }
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>

int main() {
    int n = 10, nums[] = {1, -20, 31, 14, 29, 7, -1, -4, -12, 16}, i, j, temp;
    for (i = 0; i < n; i++) {
        printf("%d ", nums[i]);
    }
    for (i = 0; i < n; i++) {
        for (j = i+1; j < n; j++) {
            // 没考虑0
            if (nums[i] < 0 && nums[j] > 0) {
                temp = nums[j];
                nums[j] = nums[i];
                nums[i] = temp;
            }
        }
    }
    printf("\n");
    for (i = 0; i < n; i++) {
        printf("%d ", nums[i]);
    }
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>

long long compose(long long m, long long n) {
    if (m > n/2) {
        m = n-m;
    }
    if (m == 1) {
        return n;
    }
    return compose(m, n-1) + compose(m-1, n-1);
}

int main() {
    printf("%lld\n", compose(2, 5));
    printf("%lld\n", compose(3, 5));
    printf("%lld\n", compose(1, 4));
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>
#include <string.h>

void sort(char *s) {
    int i, j, flag, temp, n = strlen(s);
    for (i = n-1; i >= 1; --i) {
        flag = 0;
        for (j = 1; j <= i; ++j) {
            if (s[j-1] < s[j]) {
                temp = s[j];
                s[j] = s[j-1];
                s[j-1] = temp;
                flag = 1;
            }
        }
        if (!flag) {
            return;
        }
    }
}

void merge(char *s1, char *s2) {
    int i, j, flag, len1 = strlen(s1), len2 = strlen(s2), ptr = 0;
    char str[len1+len2];
    for (i = 0; i < len1; i++) {
        flag = 1;
        for (j = 0; j < ptr; j++) {
            if (s1[i] == str[j]) {
                flag = 0;
                break;
            }
        }
        if (flag) {
            str[ptr++] = s1[i];
        }
    }
    for (i = 0; i < len2; i++) {
        flag = 1;
        for (j = 0; j < ptr; j++) {
            if (s2[i] == str[j]) {
                flag = 0;
                break;
            }
        }
        if (flag) {
            str[ptr++] = s2[i];
        }
    }
    sort(str);
    strcpy(s1, str);
}

int main() {
    char str1[] = "abhkchabeiancaka", str2[] = "123njlasknvie456";
    merge(str1, str2);
    printf("%s\n", str1);
    return 0;
}
```

9.代码如下：
```c
#include <stdio.h>

void bubbleSort(int a[], int n) {
    int i, j, flag, temp;
    for (i = n-1; i >= 1; --i) {
        flag = 0;
        for (j = 1; j <= i; ++j) {
            if (a[j-1] < a[j]) {
                temp = a[j];
                a[j] = a[j-1];
                a[j-1] = temp;
                flag = 1;
            }
        }
        if (!flag) {
            return;
        }
    }
}

int main() {
    int n = 7, a[] = {10, 30, 100, 12, 53, 24, 65}, i;
    for (i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    bubbleSort(a, n);
    printf("\n");
    for (i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>

int main() {
    int n = 10, i, j, flag = 0, flag1, flag2, flag3, flag4, nums[10][10] = {
            {0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 0, 0, 0, 0, 0},
            {1, 0, 0, 0, 0, 0, 0, 0, 0, 0},
            {0, 1, 0, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 1, 0, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 1, 0, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 1, 0, 0, 0, 0, 0},
            {0, 0, 0, 0, 0, 1, 0, 0, 0, 0}
    };
    for (i = 0; i < n; i++) {
        flag1 = 1;
        flag2 = 1;
        flag3 = 1;
        flag4 = 1;
        for (j = 0; j < n; j++) {
            if (nums[i][j] != 0) {
                flag1 = 0;
            }
            if (nums[j][i] != 0) {
                flag2 = 0;
            }
            if (nums[i][i] != 0) {
                flag3 = 0;
            }
            if (nums[i][n-i-1] != 0) {
                flag4 = 0;
            }
        }
        if (flag1 || flag2 || flag3 || flag4) {
            printf("图像包含直线\n");
            flag = 1;
            break;
        }
    }
    if (!flag) {
        printf("图像不包含直线\n");
    }
    return 0;
}
```

[Chapter13 面向对象与C++基础](/Chapter13.md)

[返回首页](/README.md)

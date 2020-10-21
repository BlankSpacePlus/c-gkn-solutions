# 第五章 迭代计算与循环结构

1.代码如下：
```c
#include <stdio.h>

int main() {
    double sum = 0, prev = 1, next = 2, temp;
    for (int i = 0; i < 10; i++) {
        sum += (next / prev);
        temp = next;
        next = prev + next;
        prev = temp;
    }
    printf("和为%.2lf", sum);
    return 0;
}
```

2.代码如下：
```c
#include <stdio.h>

int main() {
    int num;
    printf("请输入n的值：");
    scanf("%d", &num);
    long long sum = 1, temp = 0;
    for (int i = 1; i <= num; i++) {
        temp += i;
        printf("第%d项的值是%ld\n", i, temp);
        sum *= temp;
    }
    printf("各项之积为：%ld\n", sum);
    return 0;
}
```

3.代码如下：
```c
#include <stdio.h>

int main() {
    int max = 0, min = 0, num;
    while (1) {
        scanf("%d", &num);
        if (!num) {
            break;
        } else if (num < 0) {
            continue;
        }
        if (max == 0 && min == 0) {
            max = num;
            min = num;
        }
        if (max < num) {
            max = num;
        }
        if (min > num) {
            min = num;
        }
    }
    if (max == 0 && min == 0) {
        printf("没有输入正整数！\n");
    }
    printf("输入的正整数中，最大的是%d，最小的数是%d\n", max, min);
    return 0;
}
```

4.代码如下：
```c
#include <stdio.h>

int gcd(int m, int n) {
    int temp;
    while (n > 0) {
        temp = m % n;
        m = n;
        n = temp;
    }
    return m;
}

int main() {
    int m, n;
    printf("请输入两个数，空格分隔：");
    scanf("%d %d", &m, &n);
    int gcdResult = gcd(m, n);
    printf("最大公约数为：%d\n最小公倍数为：%d\n", gcdResult, m*n/gcdResult);
    return 0;
}
```

5.代码如下：
```c
#include <stdio.h>

int main() {
    int a, b, c;
    for (int i = 100; i < 1000; i++) {
        c = i;
        a = c / 100;
        c %= 100;
        b = c / 10;
        c %= 10;
        if (i == a * a * a + b * b * b + c * c * c) {
            printf("%d是水仙花数\n", i);
        }
    }
    return 0;
}
```

6.代码如下(暴力求解)：
```c
#include <stdio.h>

int main() {
    int sum = 0;
    for (int i = 1; i <= 1000; i++) {
        for (int j = 1; j < i; j++) {
            if (i % j == 0) {
                sum += j;
            }
        }
        if (i == sum) {
            printf("%d是完数\n", i);
        }
        sum = 0;
    }
    return 0;
}
```

7.代码如下(使用了欧拉筛)：
```c
#include <stdio.h>
#include <string.h>

int n = 1000, pointer = 0;

// 欧拉筛
void euler(int *nums, int *primes) {
    for (int i = 2; i < n; i++) {
        if (!nums[i]) {
            primes[pointer++] = i;
        }
        for (int j = 0; j < pointer && i*primes[j] < n; j++) {
            nums[i*primes[j]] = 1;
            if (!i % primes[j]) {
                break;
            }
        }
    }
}

int main() {
    int nums[n+1], primes[n];
    memset(nums, 0, sizeof(nums));
    euler(nums, primes);
    printf("请输入一个1000以内的正偶数(大于4)：");
    int num;
    scanf("%d", &num);
    if (num <= 4 || num >= 1000 || (num&1)) {
        printf("输入数据不合法！");
        return -1;
    }
    for (int i = 0; i < pointer; i++) {
        for (int j = 0; j < pointer; j++) {
            if (primes[i] + primes[j] == num) {
                printf("%d = %d + %d", num, primes[i], primes[j]);
                return 0;
            }
        }
    }
    return 0;
}
```

8.代码如下(暴力算法)：
```c
#include <stdio.h>

int main() {
    int counter = 0;
    for (int i = 1; i <= 4; i++) {
        for (int j = 1; j <= 4; j++) {
            if (j == i) {
                continue;
            }
            for (int k = 1; k <= 4; k++) {
                if (k == i || k == j) {
                    continue;
                }
                for (int l = 1; l <= 4; l++) {
                    if (l == i || l == j || l == k) {
                        continue;
                    }
                    printf("%d%d%d%d\n", i, j, k, l);
                    counter++;
                }
            }
        }
    }
    printf("一共%d个\n", counter);
    return 0;
}
```

9.代码如下：
```c
#include <stdio.h>

int main() {
    for (int i = 1; i <= 5; i++) {
        for (int j = 1; j <= 5; j++) {
            for (int k = 0; k <= 9; k++) {
                if (i*100+j*10+k+j*100+k*10+k == 532) {
                    printf("xyz+yzz=532中，x=%d, y=%d, c=%d\n", i, j, k);
                    break;
                }
            }
        }
    }
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>
#include <time.h>
#include <stdlib.h>

int main() {
    srand(time(0));
    printf("猜数字游戏开始！\n");
    int times = 1, result, answer;
    char choice[1024];
    while (1) {
        printf("这是第%d次游戏\n", times);
        result = rand();
        for (int i = 1; i <= 10; i++) {
            printf("你的第%d次猜测：", i);
            scanf("%d", &answer);
            if (answer == result) {
                printf("你猜对啦！\n");
                break;
            } else if (answer < result) {
                printf("你猜低了！\n");
            } else {
                printf("你猜高了！\n");
            }
            if (i < 10) {
                printf("请继续猜！\n");
            } else {
                printf("游戏失败！答案是：%d\n", result);
            }
        }
        printf("是否继续猜数字(Y/N)？");
        scanf("%s", &choice);
        while (choice[0] != 'N' && choice[0] != 'n' && choice[0] != 'Y' && choice[0] != 'y') {
            printf("输入数据不合法！\n");
            printf("是否继续猜数字(Y/N)？");
            scanf("%s", &choice);
        }
        if (choice[0] == 'N' || choice[0] == 'n') {
            break;
        }
        times++;
    }
    printf("游戏结束！\n");
    return 0;
}
```

[Chapter06 集合数据与数组](/Chapter06.md)

[返回首页](/README.md)

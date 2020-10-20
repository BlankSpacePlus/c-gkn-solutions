# 第四章 逻辑判断与选择结构

1.(逻辑运算符优先级：非>与>或)<br/>
1. `!a||a`：1(TRUE)
2. `a&&!a`：0(FALSE)
3. `!a||(a&&1)`：1(TRUE)
4. `a&&(!a||1)`：1(TRUE)

2.语句如下：
1. `x<z ^ y<z`
2. `x<0&&y<0&&z>=0 || x<0&&y>=0&&z<0 || z>=0&&y<0&&z<0`
3. `!(x&1)`

3.代码如下：
```c
#include <stdio.h>

int main() {
    int n;
    printf("请输入后推天数N：");
    scanf("%d", &n);
    char* str;
    switch ((n+5)%7) {
        case 0:
            str = "星期一";
            break;
        case 1:
            str = "星期二";
            break;
        case 2:
            str = "星期三";
            break;
        case 3:
            str = "星期四";
            break;
        case 4:
            str = "星期五";
            break;
        case 5:
            str = "星期六";
            break;
        default:
            str = "星期日";
            break;
    }
    printf("%s", str);
    return 0;
}
```

4.代码如下(暴力求解)：
```c
#include <stdio.h>
#include <stdlib.h>

// 从大到小
int cmpFunc (const void *a, const void *b) {
    return -(*(int*)a - *(int*)b);
}

int main() {
    int values[4];
    for (int i = 0; i < 4; i++) {
        scanf("%d", &values[i]);
    }
    qsort(values, 4, sizeof(int), cmpFunc);
    for (int i = 0; i < 4; i++) {
        printf("%d ", values[i]);
    }
    printf("\n");
    return 0;
}
```

5.代码如下：
```c
#include <stdio.h>
#include <math.h>

int main() {
    double x, result;
    printf("请输入函数自变量x：");
    scanf("%lf", &x);
    if (x <= 0) {
        result = 0;
    } else if (x <= 10) {
        result = x;
    } else {
        result = 0.5 + sin(x);
    }
    printf("%.2f", result);
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>
#include <ctype.h>

int main() {
    char c;
    char *result;
    printf("请输入一个字符：");
    scanf("%c", &c);
    if (isupper(c)) {
        result = "大写字母";
    } else if (islower(c)) {
        result = "小写字母";
    } else if (isdigit(c)){
        result = "数字";
    } else {
        result = "其他";
    }
    printf("%c是%s字符", c, result);
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>
#include <ctype.h>

int main() {
    double profit = 0, reward = 0;
    printf("请输入当月利润：");
    scanf("%lf", &profit);
    if (profit < 50000) {
        reward = 0;
    } else if (profit < 100000) {
        reward = 0.1 * profit;
    } else if (profit < 200000){
        reward = 0.075 * profit;
    } else if (profit < 300000){
        reward = 0.05 * profit;
    } else {
        reward = 0.02 * profit;
    }
    printf("奖金总额是：%.2lf元", reward);
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>

int main() {
    int num, a, b;
    printf("请输入一个不大于三位数的正整数：");
    scanf("%d", &num);
    a = num / 100;
    num %= 100;
    b = num / 10;
    num %= 10;
    printf("%d", a+b+num);
    return 0;
}
```

9.代码如下：
```c
#include <stdio.h>

int main() {
    int num, a, b;
    printf("请输入一个两位的正整数：");
    scanf("%d", &num);
    if (num < 10 || num > 99) {
        printf("输入数据不合法");
        return -1;
    }
    a = num / 10;
    b = num % 10;
    printf("%d", (a > b ? a+b*10 : num));
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>

int main() {
    // Monday Tuesday Wednesday Thursday Friday Saturday Sunday
    char tempChar;
    char *day;
    printf("请输入代表星期几的字符：");
    tempChar = getchar();
    switch (tempChar) {
        case 'm':
        case 'M':
            day = "星期一";
            break;
        case 'w':
        case 'W':
            day = "星期三";
            break;
        case 'f':
        case 'F':
            day = "星期五";
            break;
        case 't':
        case 'T':
            tempChar = getchar();
            if (tempChar == 'u') {
                day = "星期二";
            } else if (tempChar == 'h') {
                day = "星期四";
            } else {
                printf("输入错误！");
                return -1;
            }
            break;
        case 's':
        case 'S':
            tempChar = getchar();
            if (tempChar == 'a') {
                day = "星期六";
            } else if (tempChar == 'u') {
                day = "星期日";
            } else {
                printf("输入错误！");
                return -1;
            }
            break;
        default:
            printf("输入错误！");
            return -1;
    }
    printf("你输入的是%s", day);
    return 0;
}
```

# 第一章 计算机及程序设计概述

1.略

2.略

3.略

4.代码如下：
```c
#include <stdio.h>

int main() {
    printf("请输入摄氏温度℃：");
    double temperature;
    scanf("%lf", &temperature);
    printf("对应的华氏温度是：%.2lf℉\n", 1.8*temperature+32);
    return 0;
}
```

5.代码如下：(注意这里使用了位运算优化，朴素算法是`num%2==0`)
```c
#include <stdio.h>

int main() {
    printf("请输入一个整数：");
    int num;
    scanf("%d", &num);
    printf("该整数%s一个偶数\n", ((num&1)==0?"是":"不是"));
    return 0;
}
```

6.代码如下(此代码没有考虑浮点误差)：
```c
#include <stdio.h>

int main() {
    printf("请输入待判断的三条边长：");
    double a, b, c;
    scanf("%lf %lf %lf", &a, &b, &c);
    printf("这三条边%s构成一个三角形\n", ((a+b>c&&a+c>b&&b+c>a)?"能":"不能"));
    return 0;
}
```

7.代码如下(没考虑浮点保留位数)：<br/>
(1)
```c
#include <stdio.h>

int main() {
    printf("请输入两个数值，以空格分隔：");
    double a, b;
    scanf("%lf %lf", &a, &b);
    printf("最大的数是：%lf\n", (a>=b?a:b));
    return 0;
}
```
(2)
```c
#include <stdio.h>

int main() {
    printf("请输入三个数值，以空格分隔：");
    double a, b, c;
    scanf("%lf %lf %lf", &a, &b, &c);
    printf("最大的数是：%lf\n", (a>=b&&a>=c?a:b>=a&&b>=c?b:c));
    return 0;
}
```
(3)(切记max初始值不能定义为0，否则就相当于全负数会WA掉)
```c
#include <stdio.h>

int main() {
    printf("请输入10个数值，以空格分隔：");
    // 注意不需要使用10个数值或者1个长度为10数组，毕竟我们只需要求出最大值而已
    double max, temp;
    scanf("%lf", &max);
    for (int i = 1; i < 10; i++) {
        scanf("%lf", &temp);
        if (temp > max) {
            max = temp;
        }
    }
    printf("最大的数是：%lf\n", max);
    return 0;
}
```
(4)自行补充：输入任意数量的数值，找出max(下面这个程序的不足是如果输入空格再回车会程序会RE掉)
```c
#include <stdio.h>

int main() {
    printf("请输入任意个数值，以空格分隔，以回车结束(回车前请不要留有空格)：");
    double max = 0, temp;
    while (1) {
        scanf("%lf", &temp);
        if (temp > max) {
            max = temp;
        }
        if (getchar() == '\n') {
            break;
        }
    }
    printf("最大的数是：%lf\n", max);
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>

int main() {
    printf("请输入10个数值，以空格分隔：");
    double sum = 0, temp;
    for (int i = 0; i < 10; i++) {
        scanf("%lf", &temp);
        sum += temp;
    }
    printf("这10个数的总和是：%lf\n", sum);
    return 0;
}
```

9.代码如下(引入了基本的范围检查)：
```c
#include <stdio.h>

int main() {
    printf("请输入一个三位整数：");
    int n, a, b, c;
    scanf("%d", &n);
    if (n < 100 || n > 999) {
        printf("输入错误！\n");
        return -1;
    }
    c = n;
    a = c / 100;
    c %= 100;
    b = c / 10;
    c %= 10;
    printf("三位数%d的百位数字是%d十位数字是%d个位数字是%d\n", n, a, b, c);
    return 0;
}
```

10.代码如下(引入了基本的范围检查)：
```c
#include <stdio.h>

int main() {
    printf("请输入一个年份：");
    int year;
    scanf("%d", &year);
    if (year < 0) {
        printf("输入年份错误！\n");
        return -1;
    }
    printf("%d年%s闰年\n", year, (year%400==0 || (year%100!=0 && year%4==0) ? "是" : "不是"));
    return 0;
}
```

[Chapter02 信息编码与数据类型](/Chapter02.md)

[返回首页](/README.md)

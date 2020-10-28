# 第十章 泛化编程与预编译

1.略

2.？？？

3.代码段如下：
```c
#ifdef MY_DOS
    typedef long INT32;
#elif MY_WIN32
    typedef int INT32;
#endif
```

4.说明如下：<br/>
N定义为10，M1和M2都是N*2，所以都是20，x=M1+M2，所以x是40。

5.代码如下：
```c
#include <stdio.h>

#define ADD(a,b) c=a+b;
#define SUBTRACT(a,b) c=a-b;
#define MULTIPLY(a,b) c=a*b;
#define DIVIDE(a,b) c=a/b;

int main() {
    int a = 4, b = 2, c;
    ADD(a, b);
    printf("%d + %d = %d\n", a, b, c);
    SUBTRACT(a, b);
    printf("%d - %d = %d\n", a, b, c);
    MULTIPLY(a, b);
    printf("%d * %d = %d\n", a, b, c);
    DIVIDE(a, b);
    printf("%d / %d = %d\n", a, b, c);
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>

#define MAX(a,b,c) max=(a>b&&a>c)?a:(b>a&&b>c)?b:c;

double max_fun(double a, double b, double c) {
    return (a>b&&a>c)?a:(b>a&&b>c)?b:c;
}

int main() {
    double a, b, c, max;
    scanf("%lf %lf %lf", &a, &b, &c);
    MAX(a, b, c);
    printf("%.3lf\n%.3lf", max_fun(a,b,c), max);
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>

#define SWAP(a,b) t=a; a=b; b=t;

int main() {
    int x,y,t;
    scanf("%d %d", &x, &y);
    SWAP(x,y);
    printf("%d %d", x, y);
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>

#include "format.h"

int main() {
    printf(INT_FORMAT, 200);
    printf(FLOAT_FORMAT, 3.14159);
    printf(STRING_FORMAT, "Hello");
    return 0;
}
```

其中的`format.h`：
```c
#ifndef TEST_FORMAT_H
#define TEST_FORMAT_H

#define INT_FORMAT "%d\n"
#define FLOAT_FORMAT "%.2f\n"
#define STRING_FORMAT "%s\n"

#endif //TEST_FORMAT_H
```

9.代码如下：
```c
#include <stdio.h>

#define LCOUNT(a,s,n,c) c=0; for(int i=0; i<n; i++){if(a[i]>s){c++;}}

int main() {
    int count = 0, int_arr[10] = {1, 2, 3, 3, 7, 77, 777, 7777, 4396, 2200}, n=10;
    char char_arr[10] = "AGDCAOMSMF";
    float float_arr[10] = {1.0F, 2.0F, 4.5F, 5.0F, 4.9F, 4.1F, 4.0F, 3.9F, 4.15F, 4.2F};
    LCOUNT(int_arr, 7, n, count);
    printf("%d\n", count);
    LCOUNT(float_arr, 3.14, n, count);
    printf("%d\n", count);
    LCOUNT(char_arr, 'H', n, count);
    printf("%d\n", count);
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main() {
    char line[30] = "AGDCAOMSMFscaFFaCASsa";
    int counter = 0;
    for (int i = 0; i < strlen(line); i++) {
#ifdef  DEBUG
        if (isupper(line[i])) {
            counter++;
            printf("当前检测到第%d个大写字母%c", counter, line[i]);
        }
#endif
    }
    return 0;
}
```

# 第六章 集合数据与数组

1.不相同。<br/>
a[10]里的a是一维数组，而a[2][5]中的a是二维数组。

2.代码如下：
```c
#include <stdio.h>

int main() {
    printf("请输入输入整数的个数N：");
    int n, k, m;
    scanf("%d", &n);
    int nums[n];
    printf("请输入整数序列：");
    for (int i = 0; i < n; i++) {
        scanf("%d", &nums[i]);
    }
    printf("请输入待插入的整数K：");
    scanf("%d", &k);
    // 插入位置按照从0开始算
    printf("请输入待插入位置：");
    scanf("%d", &m);
    int b[n+1];
    for (int i = 0; i < m; i++) {
        b[i] = nums[i];
    }
    printf("%d\n", m);
    b[m] = k;
    for (int i = m+1; i < n+1; i++) {
        b[i] = nums[i-1];
    }
    // 打印数组
    for (int i = 0; i < n+1; i++) {
        printf("%d ", b[i]);
    }
    return 0;
}
```

3.代码如下：
```c
#include <stdio.h>

int main() {
    int n = 10, pointer = 0, flag = 1;
    printf("原数组：");
    int a[] = {1, 2, 1, 2, 3, 5, 3, 3, 6, 4};
    for (int i = 0; i < n; i++) {
        printf("%d ", a[i]);
    }
    int b[n];
    b[pointer++] = a[0];
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < pointer; j++) {
            if (b[j] == a[i]) {
                flag = 0;
            }
        }
        if (flag) {
            b[pointer++] = a[i];
        } else {
            flag = 1;
        }
    }
    printf("\n去重以后的数组：");
    for (int i = 0; i < pointer; i++) {
        printf("%d ", b[i]);
    }
    return 0;
}
```

4.代码如下：
```c
#include <stdio.h>

int main() {
    int a[10] = {1, 3, 5, 6, 9, 133, 155, 199, 2333, 777777};
    int b[5] = {0, 4, 1000, 10000, 1000000};
    int c[15];
    int i = 0, j = 0;
    while (1) {
        if (i == 10) {
            if (j == 5) {
                break;
            }
            c[i+j] = b[j];
            j++;
        } else if (j == 5) {
            c[i+j] = a[i];
            i++;
        } else {
            if (a[i] <= b[j]) {
                c[i+j] = a[i];
                i++;
            } else {
                c[i+j] = b[j];
                j++;
            }
        }
    }
    printf("数组a：");
    for (i = 0; i < 10; i++) {
        printf("%d ", a[i]);
    }
    printf("\n数组b：");
    for (i = 0; i < 5; i++) {
        printf("%d ", b[i]);
    }
    printf("\n合并后的数组c：");
    for (i = 0; i < 15; i++) {
        printf("%d ", c[i]);
    }
    return 0;
}
```

5.代码如下：
```c
#include <stdio.h>

int main() {
    int a[4][4] = {{1, 2, 3, 4}, {2, 3, 4, 5}, {3, 4, 5, 6}, {4, 5, 6, 7}};
    printf("原始矩阵：\n");
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            printf("%d ", a[i][j]);
        }
        printf("\n");
    }
    int sum = 0;
    for (int i = 0; i < 4; i++) {
        sum += a[i][i];
        sum += a[3-i][i];
    }
    printf("两对角线元素之和：%d", sum);
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>

int main() {
    int a[4][5] = {{1, 2, 3, 4, 5}, {2, 3, 4, 5, 6}, {3, 4, 5, 6, 7}, {4, 5, 6, 7, 8}};
    int max_i = 0, max_j = 0, min_i = 0, min_j = 0, max = a[0][0], min = a[0][0];
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 5; j++) {
            if (a[i][j] > max) {
                max = a[i][j];
                max_i = i;
                max_j = j;
            }
            if (a[i][j] < min) {
                min = a[i][j];
                min_i = i;
                min_j = j;
            }
        }
    }
    printf("矩阵元素最大值是：%d，它位于第%d行第%d列\n矩阵元素最小值是：%d，它位于第%d行第%d列\n", max, max_i, max_j, min, min_i, min_j);
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>

int main() {
    int n = 3, flag = 1;
    int a[3][3] = {{4, 9, 2}, {3, 5, 7}, {8, 1, 6}};
    int sum = 0, temp = 0;
    for (int i = 0; i < n; i++) {
        sum += a[i][0];
    }
    for (int i = 1; i < n; i++) {
        for (int j = 0; j < n; j++) {
            temp += a[i][j];
        }
        if (temp != sum) {
            flag = 0;
            break;
        }
        temp = 0;
    }
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            temp += a[j][i];
        }
        if (temp != sum) {
            flag = 0;
            break;
        }
        temp = 0;
    }
    for (int i = 0; i < n; i++) {
        temp += a[i][i];
    }
    if (temp != sum) {
        flag = 0;
    }
    temp = 0;
    for (int i = 0; i < n; i++) {
        temp += a[n-1-i][i];
    }
    if (temp != sum) {
        flag = 0;
    }
    printf("此矩阵%s幻方\n", (flag ? "是" : "不是"));
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>

int main() {
    char str[] = "scnasionsanalgnlkdangklgnlk";
    char ch = 'l';
    printf("原字符串为：%s\n", str);
    char temp[30];
    int pointer = 0;
    char c = str[0];
    for (int i = 0; c != '\0'; i++) {
        c = str[i];
        if (str[i] != ch) {
            temp[pointer] = str[i];
            pointer++;
        }
    }
    printf("新字符串为：%s\n", temp);
    return 0;
}
```

9.代码如下：
```c
#include <stdio.h>

int main() {
    char str[] = "abcdefghijklmnopqrstuvwxyz";
    char append[] = "123456";
    printf("原字符串为：%s\n待插入字符串为：%s\n", str, append);
    char result[50];
    int append_index = 10;
    for (int i = 0; i < append_index; i++) {
        result[i] = str[i];
    }
    char c = append[0];
    int pointer = append_index;
    for (int i = 0; c != '\0'; i++) {
        c = append[i];
        result[pointer] = append[i];
        pointer++;
    }
    // 多加了一次=>补偿一次
    pointer--;
    c = str[append_index];
    for (int i = append_index; c != '\0'; i++) {
        c = str[i];
        result[pointer] = c;
        pointer++;
    }
    printf("新字符串为：%s\n", result);
    return 0;
}
```

10.代码如下(可以优化的地方是关于(s)这里是去括号还是整个删去的细节，不过题目中似乎并没有强调这一点)：
```c
#include <stdio.h>

int main() {
    char str[] = "0011111100010100000111";
    int counter = 1;
    char temp = str[0], c = str[0];
    for (int i = 1; c != '\0'; i++) {
        c = str[i];
        if (c == temp) {
            counter++;
        } else {
            printf("Emit %c for %d time unit(s)\n", temp, counter);
            temp = c;
            counter = 1;
        }
    }
    if (counter > 1) {
        printf("Emit %c for %d time unit(s)\n", temp, counter);
    }
    return 0;
}
```

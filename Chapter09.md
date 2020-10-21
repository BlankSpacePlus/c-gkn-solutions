# 第九章 复杂数据类型与结构体

1.略

2.略

3.略

4.代码如下：
```c
#include <stdio.h>

struct teacher {
    int card_id;
    char name[20];
    char sex[10];
    int birthYear;
    int birthMonth;
    union {
        // 1->未婚, 2->已婚, 3->离异
        int stateNum;
        char state[10];
    } married;
    char company[50];
};

int main() {
    struct teacher t1 = {123, "Sam", "男", 1990, 11, 2, "THU"};
    printf("工资卡号：%d\n", t1.card_id);
    printf("姓名：%s\n", t1.name);
    printf("性别：%s\n", t1.sex);
    printf("出生年份：%d\n", t1.birthYear);
    printf("出生月份：%d\n", t1.birthMonth);
    printf("婚姻状态：%s\n", (t1.married.stateNum == 1 ? "未婚" : t1.married.stateNum == 2 ? "已婚" : "离异"));
    printf("工作部门：%s\n", t1.company);
    return 0;
}
```

5.代码如下：
```c
#include <stdio.h>

struct date {
    int year;
    int month;
    int day;
};

int main() {
    struct date d1 = {2020, 11, 11}, d2 = {1990, 12, 5};
    printf("年份差值：%d\n", d1.year-d2.year);
    printf("月份差值：%d\n", d1.month-d2.month);
    printf("日期差值：%d\n", d1.day-d2.day);
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>

struct student {
    int id;
    char name[20];
    int grade;
};

int main() {
    struct student s1 = {101, "Sam", 90}, s2 = {102, "Bob", 60},
            s3 = {103, "Amy", 100}, s4 = {104, "Tim", 97},
            s5 = {105, "Jessica", 95};
    double average = (s1.grade + s2.grade + s3.grade + s4.grade + s5.grade) / 5.0;
    if (s1.grade > average) {
        printf("学生ID：%d，学生姓名：%s，学生成绩：%d\n", s1.id, s1.name, s1.grade);
    }
    if (s2.grade > average) {
        printf("学生ID：%d，学生姓名：%s，学生成绩：%d\n", s2.id, s2.name, s2.grade);
    }
    if (s3.grade > average) {
        printf("学生ID：%d，学生姓名：%s，学生成绩：%d\n", s3.id, s3.name, s3.grade);
    }
    if (s4.grade > average) {
        printf("学生ID：%d，学生姓名：%s，学生成绩：%d\n", s4.id, s4.name, s4.grade);
    }
    if (s5.grade > average) {
        printf("学生ID：%d，学生姓名：%s，学生成绩：%d\n", s5.id, s5.name, s5.grade);
    }
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>
#include <stdlib.h>

struct student {
    int id;
    char name[20];
    int math;
    int english;
    int program;
};

int cmpFunc (const void *a, const void *b) {
    struct student *p1 = (struct student*)a, *p2 = (struct student*)b;
    return -((p1->math + p1->english + p1->program) - (p2->math + p2->english + p2->program));
}

int main() {
    struct student s1 = {101, "Sam", 20, 30, 40},
            s2 = {102, "Bob", 60, 60, 60},
            s3 = {103, "Amy", 70, 70, 70},
            s4 = {104, "Tim", 97, 98, 99},
            s5 = {105, "Jessica", 95, 90, 98},
            s6 = {106, "Robin", 60, 60, 34},
            s7 = {107, "Rust", 100, 100, 100},
            s8 = {108, "Python", 97, 99, 95},
            s9 = {109, "Java", 95, 99, 80},
            s10 = {110, "Swift", 95, 77, 66};
    struct student list[] = {s1, s2, s3, s4, s5, s6, s7, s8, s9, s10}, temp;
    qsort(list, 10, sizeof(struct student), cmpFunc);
    for (int i = 0; i < 10; i++) {
        temp = list[i];
        printf("学生%d的个人信息：\n学号：%d\t姓名：%s\t数学成绩：%d\t英语成绩：%d\t编程成绩：%d\n",
                i+1, temp.id, temp.name, temp.math, temp.english, temp.program);
    }
    return 0;
}
```

8.代码如下(用了随机数来模拟)：
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

struct player {
    int id;
    char name[30];
    char nationality[30];
    double avg_point;
};

int cmpFunc (const void *a, const void *b) {
    struct player *p1 = (struct player*)a, *p2 = (struct player*)b;
    double temp = p1->avg_point - p2->avg_point;
    return temp > 0 ? -1 : temp == 0 ? 0 : 1;
}

int main() {
    srand(time(0));
    double grades[10][7], avg_arr[10],temp, max, min;
    int i, j;
    for (i = 0; i < 10; i++) {
        for (j = 0; j < 7; j++) {
            grades[i][j] = (int)rand() % 11;
        }
    }
    for (i = 0; i < 10; i++) {
        temp = 0;
        max = grades[i][0];
        min = max;
        for (j = 0; j < 7; j++) {
            temp += grades[i][j];
            if (grades[i][j] > max) {
                max = grades[i][j];
            }
            if (grades[i][j] < min) {
                min = grades[i][j];
            }
        }
        temp -= max;
        temp -= min;
        avg_arr[i] = temp / 5;
    }
    struct player
            p1 = {101, "Sam", "中国", avg_arr[0]},
            p2 = {102, "Bob", "美国", avg_arr[1]},
            p3 = {103, "Amy", "英国", avg_arr[2]},
            p4 = {104, "Tim", "法国", avg_arr[3]},
            p5 = {105, "Jessica", "德国", avg_arr[4]},
            p6 = {106, "Robin", "韩国", avg_arr[5]},
            p7 = {107, "Rust", "日本", avg_arr[6]},
            p8 = {108, "Python", "苏俄", avg_arr[7]},
            p9 = {109, "Java", "古巴", avg_arr[8]},
            p10 = {110, "Swift", "尤里", avg_arr[9]};
    struct player list[] = {p1, p2, p3, p4, p5, p6, p7, p8, p9, p10}, temp_p;
    qsort(list, 10, sizeof(struct player), cmpFunc);
    for (i = 0; i < 10; i++) {
        temp_p = list[i];
        printf("选手%d的个人信息：\n编号：%d\t姓名：%s\t国籍：%s\t总评：%lf\n",
                i+1, temp_p.id, temp_p.name, temp_p.nationality, temp_p.avg_point);
    }
    return 0;
}
```

9.代码如下(忽略键盘输入的过程)：
```c
#include <stdio.h>
#include <stdlib.h>

struct student {
    int id;
    char name[20];
    int grade;
};

int *defineArray(int n) {
    return (int *)malloc(sizeof(int) * n);
}

void freeArray(int *p) {
    free(p);
}

int main() {
    int n = 5;
    struct student
            s1 = {101, "Sam", 90},
            s2 = {102, "Bob", 60},
            s3 = {103, "Amy", 100},
            s4 = {104, "Tim", 97},
            s5 = {105, "Jessica", 95};
    struct student list[] = {s1, s2, s3, s4, s5}, temp;
    int *grades = defineArray(n), sum = 0;
    for (int i = 0; i < n; i++) {
        grades[i] = list[i].grade;
    }
    for (int i = 0; i < n; i++) {
        sum += grades[i];
    }
    double avg = (double)sum / n;
    freeArray(grades);
    printf("平均成绩：%.2lf\n", avg);
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>

struct people {
    int id;
    char name[30];
    struct people *next;
};

int main() {
    struct people
            p1  = {1,  "Sam"},
            p2  = {2,  "Bob"},
            p3  = {3,  "Amy"},
            p4  = {4,  "Tim"},
            p5  = {5,  "Jessica"},
            p6  = {6,  "Robin"},
            p7  = {7,  "Rust"},
            p8  = {8,  "Python"},
            p9  = {9,  "Java"},
            p10 = {10, "Swift"};
    p1.next  = &p2;
    p2.next  = &p3;
    p3.next  = &p4;
    p4.next  = &p5;
    p5.next  = &p6;
    p6.next  = &p7;
    p7.next  = &p8;
    p8.next  = &p9;
    p9.next  = &p10;
    p10.next = &p1;
    struct people list[] = {p1, p2, p3, p4, p5, p6, p7, p8, p9, p10}, *temp_p = &p1, *prev_p = &p10;
    int n = 10, m = 3, i;
    m %= n;
    while (temp_p->next != NULL && temp_p->next->id != temp_p->id) {
        for (i = 1; i < m; i++) {
            prev_p = temp_p;
            temp_p = temp_p->next;
        }
        printf("报数：%d\n", temp_p->id);
        temp_p = temp_p->next;
        prev_p->next = temp_p;
    }
    printf("报数：%d\n", temp_p->id);
    return 0;
}
```

# 第十一章 数据存储与文件

## 读书笔记
1.对文件进行读写操作时，要遵循“先打开，再读写，最后关闭”的原则。
- 打开文件的实质是建立文件相关信息的“文件信息区”，并使文件指针指向该区域以便进行读写操作。
- 关闭文件的实质是断开指针与文件之间的联系，即禁止对该文件进行再操作。

## 课后习题

1.略

2.代码如下：
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main() {
    FILE *fp;
    char text[1000];
    fp = fopen("alpha.txt", "a+");
    if (fp == NULL) {
        printf("文件打开失败");
        return 0;
    }
    fgets(text, 10000, fp);
    for (int i = 0; i < strlen(text); i++) {
        if (islower(text[i])) {
            text[i] -= ('a' - 'A');
        }
    }
    fputs(text, fp);
    if (fclose(fp) != 0) {
        printf("文件关闭失败！");
    }
    return 0;
}
```

3.代码如下(假设-1为数值终止符，空格分隔数字)：
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main() {
    FILE *fp;
    int temp, n, counter = 0;
    scanf("%d", &n);
    fp = fopen("data.txt", "r");
    if (fp == NULL) {
        printf("文件打开失败");
        return 0;
    }
    fscanf(fp, "%d", &temp);
    while (temp != -1) {
        if (temp == n) {
            break;
        }
        counter++;
        fscanf(fp, "%d", &temp);
    }
    if (fclose(fp) != 0) {
        printf("文件关闭失败！");
    }
    // 从0开始算
    printf("%d", counter);
    return 0;
}
```

4.代码如下：
```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

int main() {
    FILE *fp;
    fp = fopen("dict.txt", "r");
    if (fp == NULL) {
        printf("文件打开失败");
        return 0;
    }
    char buf[255][255];
    int counter = 0, punctuation = 0, len;
    for (int i = 0; !feof(fp); i++) {
        for (int j = 0; buf[i][j]!='\n'; j++){
            if (fscanf(fp, "%s", buf[0]) == 1) {
                counter++;
                len = strlen(buf[0]);
                for (int k = 0; k < len; k++) {
                    if (ispunct(buf[0][k])) {
                        punctuation++;
                    }
                }
            }
        }
    }
    if (fclose(fp) != 0) {
        printf("文件关闭失败！");
    }
    printf("单词数：%d\n标点符号数：%d\n", counter, punctuation);
    return 0;
}
```

测试数据文件：
```text
Hello, Hi, I am LiHua!
Oh! I am LiHua!
```

5.代码如下：
```c
#include <stdio.h>

int main() {
    FILE *fp;
    int ptr = 0;
    char filename[50], text[1000], temp;
    scanf("%s", filename);
    fp = fopen(filename, "w");
    if (fp == NULL) {
        printf("文件打开失败");
        return 0;
    }
    // 排除第一行这个回车
    getchar();
    // 真正有用的
    temp = getchar();
    while (temp != '#') {
        text[ptr] = temp;
        ptr++;
        temp = getchar();
    }
    fprintf(fp, "%s", text);
    if (fclose(fp) != 0) {
        printf("文件关闭失败！");
    }
    return 0;
}
```

6.代码如下：
```c
#include <stdio.h>
#include <string.h>

int main() {
    FILE *fpa, *fpb, *fpc;
    char a[1000], b[1000], c[1000];
    fpa = fopen("a.txt", "r");
    fpb = fopen("b.txt", "r");
    fpc = fopen("c.txt", "w");
    if (fpa == NULL || fpb == NULL || fpc == NULL) {
        printf("文件打开失败");
        return 0;
    }
    fgets(a, 10000, fpa);
    fgets(b, 10000, fpb);
    int len_a = strlen(a), len_b = strlen(b);
    for (int i = 0; i < len_a; i++) {
        c[i] = a[i];
    }
    for (int i = 0; i < len_b; i++) {
        c[i+len_a] = b[i];
    }
    c[len_a+len_b] = '\0';
    fputs(c, fpc);
    if (fclose(fpa) != 0 || fclose(fpb) != 0 || fclose(fpc) != 0) {
        printf("文件关闭失败！");
    }
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>

typedef struct student Student;

struct student  {
    int id;
    char name[30];
    int score[3];
};

// 测试数据
// 1 Alice 90 95 100
// 2 Bob 77 76 91
// 3 Cat 99 99 99
int main() {
    int n = 3, i, j;
    double temp;
    Student students[n];
    double avg[n];
    FILE *fp;
    fp = fopen("data.txt", "a+");
    if (fp == NULL) {
        printf("文件打开失败");
        return 0;
    }
    for (i = 0; i < n; i++) {
        temp = 0;
        scanf("%d %s", &students[i].id, students[i].name);
        for (j = 0; j < 3; j++) {
            scanf("%d", &students[i].score[j]);
            temp += students[i].score[j];
        }
        avg[i] = temp / 3.0;
        fprintf(fp, "%d %s %d %d %d %lf\n", students[i].id, students[i].name, students[i].score[0],
                students[i].score[1], students[i].score[2], avg[i]);
    }
    if (fclose(fp) != 0) {
        printf("文件关闭失败！");
    }
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct student Student;

struct student  {
    int id;
    char name[30];
    int score[3];
};

int cmpFunc (const void *a, const void *b) {
    Student *p1 = (Student*)a, *p2 = (Student*)b;
    int a_sum = 0, b_sum = 0, i, n = 3;
    for (i = 0; i < n; i++) {
        a_sum += p1->score[i];
        b_sum += p2->score[i];
    }
    return -(a_sum - b_sum);
}

// 测试数据
// 1 Alice 90 95 100
// 2 Bob 77 76 91
// 3 Cat 99 99 99
// 4 Davis 60 60 60
// 5 Eleven 100 100 100
int main() {
    int n = 5, i, j;
    FILE *fp;

    // 把键盘输入数据写进文件里
    char filename[20] = "stud.dat";
    fp = fopen(filename, "a+");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    int temp_id, temp_score1, temp_score2, temp_score3;
    char temp_name[50];
    for (i = 0; i < n; i++) {
        scanf("%d %s %d %d %d", &temp_id, temp_name, &temp_score1, &temp_score2, &temp_score3);
        fprintf(fp, "%d %s %d %d %d\n", temp_id, temp_name, temp_score1, temp_score2, temp_score3);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
    }

    // 把数据从文件读出来再赋值给数组
    Student students[n];
    fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fscanf(fp, "%d %s %d %d %d\n", &students[i].id, students[i].name, &students[i].score[0],
               &students[i].score[1], &students[i].score[2]);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
    }

    // 数据处理（sort）
    qsort(students, n, sizeof(Student), cmpFunc);

    // 把数据从数组写进新文件里
    char new_filename[20] = "studsort.dat";
    fp = fopen(new_filename, "a+");
    if (fp == NULL) {
        printf("文件%s打开失败", new_filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fprintf(fp, "%d %s %d %d %d\n", students[i].id, students[i].name, students[i].score[0],
                students[i].score[1], students[i].score[2]);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", new_filename);
    }

    return 0;
}
```

9.代码如下(写的比较粗暴)：
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct student Student;

struct student  {
    int id;
    char name[30];
    int score[3];
};

// 测试数据
// 1 Alice 90 95 100
// 2 Bob 77 76 91
// 3 Cat 99 99 99
// 4 Davis 60 60 60
// 5 Eleven 100 100 100
int main() {
    int n = 5, i;
    Student temp;
    FILE *fp;

    // 把键盘输入数据写进文件里
    char filename[20] = "stud.dat";
    fp = fopen(filename, "a+");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        scanf("%d %s %d %d %d", &temp.id, temp.name, &temp.score[0],
              &temp.score[1], &temp.score[2]);
        fwrite(&temp, sizeof(Student), 1, fp);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }

    // 定义结构体数组 定义的大一点以保持灵活
    Student students[2*n];

    // 把数据从文件读出来再赋值给数组
    fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    printf("写入学生信息后文件内容：\n");
    for (i = 0; i < n; i++) {
        fread(&students[i], sizeof(Student), 1, fp);
        printf("%d %s %d %d %d\n", students[i].id, students[i].name, students[i].score[0],
                students[i].score[1], students[i].score[2]);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }

    // 查找id为3的学生
    int find_id = 3;
    for (i = 0; i < n; i++) {
        if (students[i].id == find_id) {
            break;
        }
    }
    printf("学号为%d的学生被存储文件的第%d个结构体中\n", find_id, i+1);

    // 末尾插入一个学生信息：6 Frank 55 55 55
    Student newStudent = {6, "France", 55, 55, 55};
    students[n] = newStudent;
    // n增加一个元素
    n++;
    // 清空原文件内容
    fp = fopen(filename, "w");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    fprintf(fp, "");
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }
    // 修改写入文件
    fp = fopen(filename, "a+");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fwrite(&students[i], sizeof(Student), 1, fp);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }
    // 查看文件内容
    printf("增加新的学生信息后文件内容：\n");
    fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fread(&temp, sizeof(Student), 1, fp);
        printf("%d %s %d %d %d\n", temp.id, temp.name, temp.score[0], temp.score[1], temp.score[2]);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }

    // 末尾删除一个学生信息
    // n减少一个元素即可，比较复杂的情况就不写了
    n--;
    // 清空原文件内容
    fp = fopen(filename, "w");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    fprintf(fp, "");
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }
    // 修改写入文件
    fp = fopen(filename, "a+");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fwrite(&students[i], sizeof(Student), 1, fp);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }
    // 查看文件内容
    printf("删除最后一个学生信息后文件内容：\n");
    fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fread(&temp, sizeof(Student), 1, fp);
        printf("%d %s %d %d %d\n", temp.id, temp.name, temp.score[0], temp.score[1], temp.score[2]);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }

    // 修改学号为3的学生的科目三成绩为90
    students[2].score[2] = 90;
    // 清空原文件内容
    fp = fopen(filename, "w");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    fprintf(fp, "");
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }
    // 修改写入文件
    fp = fopen(filename, "a+");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fwrite(&students[i], sizeof(Student), 1, fp);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }
    // 查看文件内容
    printf("修改指定学生信息后文件内容：\n");
    fp = fopen(filename, "r");
    if (fp == NULL) {
        printf("文件%s打开失败", filename);
        return 0;
    }
    for (i = 0; i < n; i++) {
        fread(&temp, sizeof(Student), 1, fp);
        printf("%d %s %d %d %d\n", temp.id, temp.name, temp.score[0], temp.score[1], temp.score[2]);
    }
    if (fclose(fp) != 0) {
        printf("文件%s关闭失败！", filename);
        return 0;
    }

    return 0;
}
```

10.代码摘自[这里](https://blog.csdn.net/qq_39400113/article/details/104750460) ：
```c
#include <stdio.h>
#include "bmpformat.h"

BITMAPFILEHEADER fileHeader;
BITMAPINFOHEADER infoHeader;

void showBmpHead(BITMAPFILEHEADER pBmpHead) {    //定义显示信息的函数，传入文件头结构体
    printf("BMP文件大小：%dkb\n", fileHeader.bfSize/1024);
    printf("保留字必须为0：%d\n",  fileHeader.bfReserved1);
    printf("保留字必须为0：%d\n",  fileHeader.bfReserved2);
    printf("实际位图数据的偏移字节数: %d\n",  fileHeader.bfOffBits);
}

void showBmpInfoHead(BITMAPINFOHEADER pBmpinfoHead){    //定义显示信息的函数，传入的是信息头结构体
    printf("位图信息头:\n" );
    printf("信息头的大小:%d\n" ,infoHeader.biSize);
    printf("位图宽度:%d\n" ,infoHeader.biWidth);
    printf("位图高度:%d\n" ,infoHeader.biHeight);
    printf("图像的位面数(位面数是调色板的数量,默认为1个调色板):%d\n" ,infoHeader.biPlanes);
    printf("每个像素的位数:%d\n" ,infoHeader.biBitCount);
    printf("压缩方式:%d\n" ,infoHeader.biCompression);
    printf("图像的大小:%d\n" ,infoHeader.biSizeImage);
    printf("水平方向分辨率:%d\n" ,infoHeader.biXPelsPerMeter);
    printf("垂直方向分辨率:%d\n" ,infoHeader.biYPelsPerMeter);
    printf("使用的颜色数:%d\n" ,infoHeader.biClrUsed);
    printf("重要颜色数:%d\n" ,infoHeader.biClrImportant);
}

int main() {
    FILE* fp;
    fp = fopen("1.bmp", "rb");//读取同目录下的image.bmp文件。
    if(fp == NULL) {
        printf("打开'image.bmp'失败！\n");
        return -1;
    }
    //如果不先读取bifType，根据C语言结构体Sizeof运算规则——整体大于部分之和，从而导致读文件错位
    unsigned short  fileType;
    fread(&fileType,1,sizeof (unsigned short), fp);
    if (fileType = 0x4d42) {
        printf("文件类型标识正确!" );
        printf("\n文件标识符：%d\n", fileType);
        fread(&fileHeader, 1, sizeof(BITMAPFILEHEADER), fp);
        showBmpHead(fileHeader);
        fread(&infoHeader, 1, sizeof(BITMAPINFOHEADER), fp);
        showBmpInfoHead(infoHeader);
        fclose(fp);
    }
    return 0;
}
```

下面是`bmpformat.h`：
```c
#ifndef TEST_BMPFORMAT_H
#define TEST_BMPFORMAT_H

/**
 * BMP格式
 * 这种格式内的数据分为三到四个部分，依次是：
 * 文件信息头 （14字节）存储着文件类型，文件大小等信息
 * 图片信息头 （40字节）存储着图像的尺寸，颜色索引，位平面数等信息调色板 （由颜色索引数决定）【可以没有此信息】
 * 位图数据 （由图像尺寸决定）每一个像素的信息在这里存储
 *
 * 一般的bmp图像都是24位，也就是真彩。每8位为一字节，24位也就是使用三字节来存储每一个像素的信息，三个字节对应存放r，g，b三原色的数据，
 * 每个字节的存贮范围都是0-255。那么以此类推，32位图即每像素存储r，g，b，a（Alpha通道，存储透明度）四种数据。8位图就是只有灰度这一种信息，
 * 还有二值图，它只有两种颜色，黑或者白。
 */
typedef struct tagBITMAPFILEHEADER {    // 文件信息头结构体
    //unsigned short bfType;        // 19778，必须是BM字符串，对应的十六进制为0x4d42,十进制为19778，否则不是bmp格式文件
    unsigned int   bfSize;        // 文件大小 以字节为单位(2-5字节)
    unsigned short bfReserved1;   // 保留，必须设置为0 (6-7字节)
    unsigned short bfReserved2;   // 保留，必须设置为0 (8-9字节)
    unsigned int   bfOffBits;     // 从文件头到像素数据的偏移  (10-13字节)
} BITMAPFILEHEADER;

typedef struct tagBITMAPINFOHEADER {    // 图像信息头结构体
    unsigned int    biSize;          // 此结构体的大小 (14-17字节)
    long            biWidth;         // 图像的宽  (18-21字节)
    long            biHeight;        // 图像的高  (22-25字节)
    unsigned short  biPlanes;        // 表示bmp图片的平面属，显然显示器只有一个平面，所以恒等于1 (26-27字节)
    unsigned short  biBitCount;      // 一像素所占的位数，一般为24   (28-29字节)
    unsigned int    biCompression;   // 说明图象数据压缩的类型，0为不压缩。 (30-33字节)
    unsigned int    biSizeImage;     // 像素数据所占大小, 这个值应该等于上面文件头结构中bfSize-bfOffBits (34-37字节)
    long            biXPelsPerMeter; // 说明水平分辨率，用象素/米表示。一般为0 (38-41字节)
    long            biYPelsPerMeter; // 说明垂直分辨率，用象素/米表示。一般为0 (42-45字节)
    unsigned int    biClrUsed;       // 说明位图实际使用的彩色表中的颜色索引数（设为0的话，则说明使用所有调色板项）。 (46-49字节)
    unsigned int    biClrImportant;  // 说明对图象显示有重要影响的颜色索引的数目，如果是0，表示都重要。(50-53字节)
} BITMAPINFOHEADER;

typedef struct _PixelInfo {    // 24位图像素信息结构体,即调色板
    unsigned char rgbBlue;   //该颜色的蓝色分量  (值范围为0-255)
    unsigned char rgbGreen;  //该颜色的绿色分量  (值范围为0-255)
    unsigned char rgbRed;    //该颜色的红色分量  (值范围为0-255)
    unsigned char rgbReserved;// 保留，必须为0
} PixelInfo;

#endif //TEST_BMPFORMAT_H
```

[Chapter12 程序设计思想及范例](/Chapter12.md)

[返回首页](/README.md)

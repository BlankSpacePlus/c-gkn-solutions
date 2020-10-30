# 第十四章 高性能计算与并行程序设计

## 读书笔记

1.并行算法的三种设计方法：
- 直接转化法
- 全新设计法
- 参考设计法

2.并行算法设计的核心是问题分解，即将一个大问题分解成若干个相对独立的子问题。<br/>
问题分解的方法主要有以下几种：
- 均匀划分法
- 方根划分法
- 对数划分法
- 功能划分法
- 分治设计法
- 流水线技术

3.并行算法的主要设计过程：
1. 划分
2. 通信分析
3. 任务组合
4. 处理器映射

4.并行程序设计模型：
- 隐式并行
- 数据并行
- 消息传递
- 共享变量

5.编程语言的并行支持
- 设计一种专门的并行程序设计语言
- 扩展现有高级程序设计语言的语法和关键字
- 基于现有高级程序设计语言提供消息传递所需的函数库

6.消息传递机制
- 简单消息发送/接收
- 同步/异步消息传递
- 广播
- 消息散布
- 消息收集
- 消息归纳

## 例题训练

```c
#include <stdio.h>
#include "mpi.h"

int main(int argc, char *argv[]) {
    int rank;
    int size;
    MPI_Init(&argc, &argv);
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);
    printf("Hello world from process %d of %d\n", rank, size);
    MPI_Finalize();
    return 0;
}
```

## 课后习题

1.略

2.略

3.略

4.略

5.略

6.代码如下(计算结点数量不太会整，貌似是直接赋值num_process)：
```c
#include <stdio.h>

#include "mpi.h"

// 避开由mpicxx.h导致的编译错误
#define MPICH_SKIP_MPICXX

unsigned long segmenty(unsigned long n, unsigned long m, unsigned long step) {
    unsigned long y = 0;
    unsigned long i;
    for (i = n; i <= m; i+=step) {
        y += i;
    }
    return y;
}

int main(int argc, char *argv[]) {
    int my_id, num_process, i;
    unsigned long n = 1024L, temp_sum, y;
    int name_len;
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    MPI_Status status;
    // 初始化MPI环境
    MPI_Init(&argc, &argv);
    // 获得当前空间进程数量
    MPI_Comm_size(MPI_COMM_WORLD, &num_process);
    // 获得当前进程ID
    MPI_Comm_rank(MPI_COMM_WORLD, &my_id);
    // 获得进程的详细名称
    MPI_Get_processor_name(processor_name, &name_len);
    printf("全部进程数量为%d进程%d运行在计算机%s\n", num_process, my_id, processor_name);
    // 立即清空输出缓冲区，并输出缓冲区内容
    fflush(stdout);
    temp_sum = segmenty(my_id + 1, n, num_process);
    if (my_id == 0) {
        // 编号为0的进程负责从其他进程收集计算结果
        y = temp_sum;
        for (i = 1; i < num_process; i++) {
            MPI_Recv(&temp_sum, 1, MPI_UNSIGNED_LONG, i, 0, MPI_COMM_WORLD, &status);
            y += temp_sum;
        }
        printf("Sum=%lu", y);
        // 清理输出流
        fflush(stdout);
    } else {
        // 其他进程负责将计算结果发送到编号为0的进程
        MPI_Send(&temp_sum, 1, MPI_UNSIGNED_LONG, 0, 0, MPI_COMM_WORLD);
    }
    MPI_Finalize();
    return 0;
}
```

7.代码如下：
```c
#include <stdio.h>

#include "mpi.h"

// 避开由mpicxx.h导致的编译错误
#define MPICH_SKIP_MPICXX

unsigned long segmenty(unsigned long n, unsigned long m, unsigned long step) {
    unsigned long y = 0;
    unsigned long i, j, flag = 1;
    for (i = n; i <= m; i+=step) {
        for (j = 2; j < i; j++) {
            if (i % j == 0) {
                flag = 0;
                break;
            }
        }
        if (flag) {
            y += i;
        }
    }
    return y;
}

int main(int argc, char *argv[]) {
    int my_id, num_process, i;
    unsigned long n = 1048576L, temp_sum, y;
    int name_len;
    char processor_name[MPI_MAX_PROCESSOR_NAME];
    MPI_Status status;
    // 初始化MPI环境
    MPI_Init(&argc, &argv);
    // 获得当前空间进程数量
    MPI_Comm_size(MPI_COMM_WORLD, &num_process);
    // 获得当前进程ID
    MPI_Comm_rank(MPI_COMM_WORLD, &my_id);
    // 获得进程的详细名称
    MPI_Get_processor_name(processor_name, &name_len);
    printf("全部进程数量为%d进程%d运行在计算机%s\n", num_process, my_id, processor_name);
    // 立即清空输出缓冲区，并输出缓冲区内容
    fflush(stdout);
    temp_sum = segmenty(my_id + 1, n, num_process);
    if (my_id == 0) {
        // 编号为0的进程负责从其他进程收集计算结果
        y = temp_sum;
        for (i = 1; i < num_process; i++) {
            MPI_Recv(&temp_sum, 1, MPI_UNSIGNED_LONG, i, 0, MPI_COMM_WORLD, &status);
            y += temp_sum;
        }
        printf("Sum=%lu", y);
        // 清理输出流
        fflush(stdout);
    } else {
        // 其他进程负责将计算结果发送到编号为0的进程
        MPI_Send(&temp_sum, 1, MPI_UNSIGNED_LONG, 0, 0, MPI_COMM_WORLD);
    }
    MPI_Finalize();
    return 0;
}
```

8.代码如下：
```c
#include <stdio.h>
#include <omp.h>

int main() {
    int sum = 0, i = 0, nts = 0, tid = 0;
    int s[10] = {0};
    nts = omp_get_num_threads();
    #pragma omp parallel private(tid, i)
    {
        tid = omp_get_thread_num();
        for (i = tid; i <= 1024; i+=nts) {
            s[tid] += i;
        }
    }
    for (i = 0; i < nts; i++) {
        sum += s[i];
    }
    printf("sum=%d", sum);
    getchar();
    return 0;
}
```

9.代码如下：
```c
#include <stdio.h>
#include <omp.h>

int main() {
    int sum = 0, i = 0, nts = 0, tid = 0, flag = 1, j;
    int s[10] = {0};
    nts = omp_get_num_threads();
    #pragma omp parallel private(tid, i)
    {
        tid = omp_get_thread_num();
        for (i = tid; i <= 1048576; i+=nts) {
            for (j = 2; j < i; j++) {
                if (i % j == 0) {
                    flag = 0;
                    break;
                }
            }
            if (flag) {
                s[tid] += i;
            }
        }
    }
    for (i = 0; i < nts; i++) {
        sum += s[i];
    }
    printf("sum=%d", sum);
    getchar();
    return 0;
}
```

10.代码如下：
```c
#include <stdio.h>
#include <windows.h>
#include <math.h>

HANDLE evtTerminate;

static long ThreadCompleted = 0;

int maxThreadCount = 4;

typedef struct parameter Parameter;

struct parameter {
    int id;
    unsigned long start;
    unsigned long end;
    int result;
};

int segmentCompute(int start, int end) {
    int i;
    int ret = 0;
    for (i = start; i <= end; i++) {
       ret += i;
    }
    return ret;
}

unsigned long WINAPI segmentThread(LPVOID p_data) {
    Parameter *p = (Parameter *)p_data;
    p->start = segmentCompute(p->start, p->end);
    Sleep(1000);
    InterlockedIncrement(&ThreadCompleted);
    if (ThreadCompleted == maxThreadCount) {
        SetEvent(evtTerminate);
    }
    return 0;
}

int main(int argc, char *argv[]) {
    HANDLE hThread[4];
    DWORD threadId[4];
    int result = 0;
    unsigned long step = 1;
    Parameter p[4] = {{1, 1, 262144}};
    int i;
    evtTerminate = CreateEvent(NULL, FALSE, FALSE, "Terminate");
    for (i = 0; i < maxThreadCount; i++) {
        p[i].start = step;
        p[i].id = i;
        p[i].end = (i+1)*262144;
        step = p[i].end+1;
        Sleep(100);
    }
    WaitForSingleObject(evtTerminate, INFINITE);
    for (i = 0; i < 4; i++) {
        result += p[i].result;
    }
    printf("Sum=%d", result);
    CloseHandle(evtTerminate);
    return 0;
}
```

[Chapter15 个体软件过程管理](/Chapter15.md)

[返回首页](/README.md)

# Lab7 **linuxC编程基础（1）**

实验目的：

1. 熟悉并掌握 linux 下 C 语言程序设计基本方法
2. 熟悉并掌握 gcc 及常用参数编译 C/C++源程序
3. 熟悉并掌握利用 GDB 调试工具调试 C/C++程序
4. 熟悉并掌握利用 MAKE 工具维护及自动编译应用程序

实验要求：

1. 熟练使用 GCC 编译工具
2. 熟练使用 GDB 调试工具
3. 熟练使用 MAKE 工具

实验内容：

## GCC

Linux 上的 C/C++ 编译器：位置/usr/bin 或/usr/local/bin 可编译 C/C++源程序。通常在编译两个或少数几个 C/C++ 源文件时，利用 GCC 编译、连接并生成可执行文件。请验证下面简单 C 程序的编译： 设两个源文件 main.c 和 factorial.c 两个源文件，现在要编译生成一个计算阶乘的程序。 

```c
//-----factorial.c-----
int factorial(int n)
{
    return n <= 1 ? 1 : factorial(n - 1) * n;
}
```

```c
//-----main.c-----
#include <stdio.h>

int main(int argc, char const *argv[])
{
    int n;
    if (argc < 2)
    {
        printf("Usage: %s n\n", argv[0]);
        return -1;
    }
    else
    {
        n = atoi(argv[1]);
        printf("Factorial of %d is %d.\n", n, factorial(n));
    }
    return 0;
}
```

## GDB

利用 GDB 调试有错误程序：利用 gdb 调试有错误的 C/C++程序使用 gdb 调试程序之前，必须使用 -g 选项编译源文件。

```c
#include
#include

static char buff[256];
static char* string;
int main() {
    puts("Please input a string:");
    gets(string);
    printf("%s\n", string);
}
```

## make

利用 make 工具实现自动编译和工程管理。验证下面题目。
设计一个程序，程序运行时从三道题目中随机抽取一道，题存放在二维数组中。

1. 分析程序，组织文件
   1. fun_main.c 从三道题目中随机抽取一道题
   2. head_random.h 包含把函数声明和用的的库函数
   3. fun_random.c 文件中定义随机函数，返回随机数，并返回主函数
   
2. 程序代码

   ```c
   //------------fun_main.c-----------------
   #include "head_random.h"
   int main(void)
   {
       int n;
       static char str[3][80] = {"linux c 函数都可以分割成独立文件吗？", "make 工具管理器的作用是？ ", " makefile 文件是否可以使用变量？ "};
       n = f_random();
       printf("随机抽取的题号是：%d\n", n + 1);
       printf("第%d 题：", n + 1);
       puts(str[n]);
   }
   //-----------head_random.h---------------
   #include <stdio.h>
   #include <stdlib.h>
   #include <time.h>
   int f_random();
   //----------fun_random.c------------------
   #include "head_random.h"
   int f_random()
   {
       srand(time(NULL));
       int a;
       a = (rand() % 3);
       return a;
   }
   ```

3. 编辑makefile文件

4. 用make命令编译程序

5. 运行程序
# Lab4 **Vim编辑器**  

实验目的

1. 熟悉并掌握 vi 命令模式、文本编辑模式和末行模式三种工作模式之间的转换方法
2. 掌握利用 vi 新建、编辑和保存文件
3. 熟悉光标的移动、文本的插入与删除等操作
4. 掌握字符串替换，行的复制、移动、撤销和删除等操作

实验要求

1. 掌握进入和退出 vi 的方法
2. 熟悉 vi 编辑器的工作方式
3. 理解 vi 文本插入和修改命令的规则、应用

实验内容

### 基础练习

1. 利用vim新建文件file2，输入以下文本内容后，保存退出

   Hello world

   I come from HZU!

2. 打开file2文件并显示行号

3. 在file2文件的第一行后插入一行内容"I'm sorry!"，并在最后一行之后添加一行，内容如下

   Hello World!

4. 将文本之中所有的Hello替换成Goodbye

5. 把第3行复制到文本的最后，把第1，2行再移动到第4行，删除第1行和第2行，并恢复删除，不保存修改（分别用命令模式和末行模式完成）

### 编写Shell

color.sh

```shell
echo –n "which color do you like?"
read color
case "$color" in
[Bb]l??)
    echo I feel $color
    echo The sky is $color;;
[Gg]ree*)
echo $color is for trees
    echo $color is for seasick;;
red | orange)
echo $color is very warm!;;
*) 
echo no such color as $color;;
esac
echo "out of case" 
```

compute.sh

```shell
#!/bin/bash
if [ $# -ne 2 ]
then
    echo "usage: $0 mdays size " 1>&2
    exit 1
fi
if [ $1 –lt 0 –o $1 –gt 30 ]
then
    echo "mdays is out of range"
    exit 2
fi
if [ $2 –le 20 ]
then
    echo "size is out of range"
    exit 3
fi
find / -xdev –mtime $1 –size +$2 –print 
```

### 编写C

```c
//------------- factorial.c-----------------------
int factorial (int n)
{ 
    if (n <= 1)
    	return 1;
    else
    	return factorial (n - 1) * n;
} 
```

```c
//----------- main.c-----------------
#include <stdio.h>
int factorial (int n);
int main (int argc, char **argv)
{ 
    int n;
    if (argc < 2) {
        printf ("Usage: %s n\n", argv [0]); 
        return -1;
    }
    else {
        n = atoi (argv[1]);
        printf ("Factorial of %d is %d.\n", n, factorial (n));
    }
    return 0;
}
```


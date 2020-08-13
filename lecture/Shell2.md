# Shell编程（2）

## Shell编程基础

### Shell脚本的输入/输出

#### 输入命令

使用read命令来将用户的输入赋值给一个变量

```shell
read variable-name1 [variable-name2……]
```

```shell
read var
```

```shell
echo $var
```

#### 输出命令

echo默认情况下是换行标准输出语句

echo输出多个空格时必须用单引号括起

```shell
echo "please input :"
```

```shell
echo -n "please input :"
```

```shell
echo -e "please input :\c"
```

### export命令

export命令可将在shell脚本中定义的变量导出到子shell 中，并使之在子shell中有效

export1.sh

```shell
#!/bin/bash
var1="The first variable"
export var2="The second variable"
sh export2
```

export2.sh

```shell
#!/bin/bash
echo "$var1"
echo "$var2"
```

### Shell的逻辑运算

所有程序设计语言的基础是对条件进行测试判断，并根据测 试结果采取不同的操作

#### 条件测试

有两种条件测试命令，语法格式如下

- test 条件表达式

- [ 条件表达式 ]

  使用第二种方法进行条件测试时，必须在[ ]前后保 留空格，否则shell提示error

![condition](C:/Users/cxhhg/Documents/Github/Course/Linux1400140/lecture/Shell/condition.png)

注意：字符串比较时=两边有空格

![comp](C:/Users/cxhhg/Documents/Github/Course/Linux1400140/lecture/Shell/comp.png)

![file](C:/Users/cxhhg/Documents/Github/Course/Linux1400140/lecture/Shell/file.png)

#### 逻辑运算

在进行条件判断时，shell提供了复杂的逻辑运算，分别是： AND运算和OR运算

##### AND运算

AND运算符为&&，它的语法格式为

```shell
statement1 && statement2 && statement3
```

&&的作用是检查前一条命令的返回值

```shell
#!/bin/bash
touch file1
rm -f file2
if [ -f file1 ] && echo "hello" && [ -f file2 ] &&
    echo "world"; then
    echo "in if"
else
    echo "in else"
fi
exit 0
```

##### OR运算

OR允许持续执行一系列命令直到有一条命令成功为止， 其后的命令将不再被执行

OR运算符为||，它的语法格式为

```shell
statement1 || statement2 || statement3
```

||列表和&&列表很相似，只是继续执行下一条命令的条件 现在变为其前一条语句必须执行失败

```shell
#!/bin/bash
touch file1
rm -f file2
if [ -f file1 ] || echo "hello" || [ -f file2 ] ||
    echo "world"; then
    echo "in if"
else
    echo "in else"
fi
exit 0
```

### 算术运算

bash提供了3种方法对数值数据进行算术运算

1. 使用expr命令
2. 使用shell扩展$((expression))
3. 使用let命令

#### expr命令

expr命令将它的参数当作一个表达式来求值

```shell
expr expression
```

注意：在使用expr时，运算符前后要有空格，且乘法要用 “\”转义，即“\\*”的形式

```sh
var1=1
```

```shell
var1=`expr $var1 + 1`
```

```shell
echo $var1
```

####  $((expression))

该命令用于计算一个expression并返回它的值

```shell
a=2 b=3
```

```shell
echo "the result of c is $((a+b))"
```

这与x=$(...)命令不同，两对圆括号用于算术替换， 而我们之前见到的一对圆括号用于命令的执行和获取输出

#### let命令

用来求算术表达式的值，如果最后表达式的值为0，let命令 返回1；否则返回0。

```shell
let expression
```

```shell
let "a=2" "b=3"
```

```shell
let c=a*b
```

```shell
echo "the value of c is $c."
```

注意：使用let命令时，变量前的$不是必须的，乘法也 不需转义使用

## Shell控制结构

### if语句

if语句提供条件测试，根据测试的条件结果执行相应的语 句

```shell
if condition
then
	statements
else
	statements
fi
```

```shell
#!/bin/bash
#this script is about if
echo " abc is the user's name? please answer yes or no"
read name
if [ $name = "yes" ]; then
    echo "hello abc!"
else
    echo "abc isn't the user's name?"
fi
exit 0
```

### elif语句

```shell
if condition1
then
	statements
elif condition2
then
	statements
elif condition3
then
	statements ……
else
	statements
fi
```

```shell
#!/bin/bash
#this script is about elif
echo " abc is the user’s name? please answer yes or no" read name
if [ "$name" = "yes" ]; then
    echo "hello abc!"
elif [ "$name" = "no" ]; then
    echo "abc isn’t the user’s name."
else
    echo "input error!"
fi
exit 0
```

### if语句其它形式

```shell
if condition ; then
    if condition ;
    then
        if condition ; then
        	statements
        fi
    fi
fi
```

```shell
if condition1 ; then
	statements
elif condition2 ; then
	statements
elif condition3 ; then
	statements ……
else
	statements
fi
```

```sh
#!/bin/bash
echo "please input filename:"
read file
if [ -r $file ]; then
    echo "$file is an ordinary file."
elif [ -d $file ]; then
    echo "$file is an directory."
else
    echo "$file isn't an ordinary file or directory."
fi
exit 0
```

### case

case是一个多分支结构，根据变量与哪种模式匹配确定 执行相应的语句序列

```sh
case variable in
pattern1) statements;;
pattern2) statements;; 
……
pattern) statements;;
*) statements;;
esac
```

```shell
#!/bin/bash
echo "please enter the number of the week:"
read number
case $number in
1) echo "Monday" ;;
2) echo "Tuesday" ;;
3) echo "Wednsday" ;;
4) echo "Thursday" ;;
5) echo "Friday" ;;
6) echo "saturday" ;;
7) echo "Sunday" ;;
*) echo "your enter must be in 1-7." ;;
esac
```

case支持合并匹配模式，即在每一个模式中，可以使用通配 符或逻辑符号

```shell
#!/bin/bash
echo " abc is the user's name? please answer yes or no"
read name
case "$name" in
y | Y | yes | YES) echo "hello abc!" ;;
n* | N*) echo "abc isn't the user's name?" ;;
*) echo "sorry,your input isn't recognized." ;;
esac
exit 0
```

在case结构中，每个分支模式可以执行多条命令

```shell
#!/bin/bash
echo " abc is the user's name? please answer yes or no"
read name
case "$name" in
y | Y | yes | YES)
    echo "hello abc!"
    echo "yes!"
    ;;
n* | N*)
    echo "abc isn't the user's name?"
    echo "no!"
    ;;
*)
    echo "sorry,your input isn't recognized."
    echo "please answer yes or no"
    exit 1
    ;;
esac
exit 0
```

### for语句

for结构可以用来循环处理一组值，这组值可以是任 意字符串的集合

for语句的语法格式如下

```shell
for variable in values
do
	statements
done
```

```shell
#!/bin/bash
for var in hello world 123; do
    echo $var
done
exit 0
```

```shell
#!/bin/bash
for file in $(ls *_script); do
    echo $file
done
exit 0
```

for循环中的参数值也可以从命令行取得

```shell
#!/bin/bash
for var in $*; do
    echo "print the arguments $var in the
command line ."
done
exit 0
```

### while语句

```shell
while condition
do
	statements
done
```

```shell
#!/bin/bash
echo –n “please enter password:” read password
while [ “$password” != “123456” ]; do
    echo “sorry, try again” read password
done
exit 0
```

通过将while结构和数值条件结合在一起，我们就可 以让某个命令执行特定的次数

```shell
#!/bin/bash
var=1
while [ "$var" -le 10 ]; do
    echo "ok,ready!"
    let var=var+1
done
exit 0
```

### until

until语句与while语句一样，都是循环语句，但处理 方式正好相反，即当判断条件为真时，循环停止。它 的语法格式为

```shell
until condition
do
	statements
done
```

```shell
#!/bin/bash
until [ -z "$1" ]; do
    echo -n "$1 "
    shift
done
echo
exit 0
```

### break和continue语句

break命令的功能是在控制条件未满足之前，跳出for 、while或until循环

```shell
#!/bin/bash
var=0
while [ "$var" -le 5 ]; do
    var=$(($var + 1))
    if [ "$var" -gt 2 ]; then
        break
    fi
    echo -n " $var "
done
echo
exit 0
```

continue使for、while或until循环跳到下一次循环继续执行，循 环变量取循环列表中的下一个值

```shell
#!/bin/bash
echo "printing numbers 1-10 (but not 3 and 7)."
var=0
while [ $var -lt 10 ]; do
    let var=var+1
    if [ $var -eq 3 ] || [ $var -eq 7 ]; then
        continue
    fi
    echo -n "$var "
done
echo
exit 0
```

## Shell函数

shell除了可以定义变量外，还可以定义函数

定义一个shell函数，语法格式如下

```shell
function_name () {
	statements
}
```

通常将函数看成是脚本中的一段代码，在使用函数前 必须先定义该函数，使用时利用函数名直接调用

```shell
#!/bin/bash
REPEAT=3
fa() {
    echo "Now fa function is starting..."
    echo
}
fb() {
    i=0
    echo "And now the fb bebins."
    sleep 1
    while [ $i -lt $REPEAT ]; do
        echo "---functions---"
        echo "-----are-------"
        echo "------fb-------"
        echo
        let i=i+1
    done
}
echo "This main script is starting..."
fa
fb
echo "main script ended."
```






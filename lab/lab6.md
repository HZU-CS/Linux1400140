# Lab6 **Shell编程**

实验目的

1. 了解 shell 运行的基本原理
2. 掌握 shell 程序编写的基本知识
3. 掌握常用 shell 变量的用法
4. 数量掌握选择（if、case）、循环（for、while、until）、函数等 she ll 脚本程序中常用的控制结构
5. 掌握基本 shell 程序设计和实现的技能

实验要求

1. 掌握 shell 脚本中的输入和输出
2. 掌握 shell 脚本中的算数、逻辑、关系等脚本运算以及相应命令
3. 掌握 if、case 选择控制结构的语法及使用
4. 掌握 for、while、until 循环控制结构及使用
5. 掌握 shell 脚本中函数的定义及使用

实验内容

编写并运行调试下列程序，运行调试过程及程序运行结果及必要的说明记入实验报告

## 斐波那契数列

编写一个脚本，求斐波那契数列的前 10 项及总和

```shell
num1=1
num2=1
echo -n “$num1+$num2”
sum=2
for((i=1;i<=8;i++))
do
tmp=$(expr $num1 + $num2)
echo -n “+$tmp”
((num1=num2))
((num2=tmp))
sum=$(expr $sum + $tmp)
done
echo “=$sum” 
```

## 验证下面脚本执行结果

```shell
echo –n “which color do you like?”
read color
case “$color” in
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
echo “out of case” 
```

## 求和

设计一个 shell 脚本：求命令行上所有整数和

```shell
sum=0
while [ $# != 0 ]
do
 let sum=sum+$1
 shift
done
echo “the sum of arguments is $sum” 
```

##  判断文件

用 Shell 编 程 ， 判 断 一 文 件 是 不 是 普 通 文 件 ， 如 果 是 将 其 拷 贝 到 /home/student/test 目录下,用命令查看 test 目录内容，最后再删除 test 目录

```shell
mkdir -m 777 /home/test
echo “Input file name:”
read FILENAME
if [ -f “$FILENAME” ]
then
cp $FILENAME /home/test
ls /home/test
cd test
rm $FILENAME
cd .
ls
else
echo”The FILENAME is not a regular file!”
fi 
```

## 模拟考勤

用 shell 设计一个模拟考勤程序，实现如下功能选择界面

1：上班签到 2：下班签出 3：缺勤信息查阅

```shell
#! /bin/bash
#filename:shelltest
exsig=0
while true; do
 echo ""
 echo "----欢迎使用本系统----"
 echo " 1. 上班签到"
echo " 2. 下班签出" 
echo " 3. 考勤信息查询"
 echo " 4. 退出系统"
 echo "----------------------"
 echo ""
 echo "请输入你的选项:"
 read choice
 case $choice in
 1)echo "请输入你的名字:"
 read name
 echo "请输入你的密码:"
 read password
 if test -r /home/user/userinfo.dat
 then
 while read fname fpassword
 do
 echo "$fname"
 echo "$fpassword"
 if test "$fname" = "$name"
 then
 break
 fi
 done < /home/user/userinfo.dat
 else
 echo System Error:userinfo.dat does not exist!
 fi
 if test "$fname" != "$name"
 then
 echo "不存在该用户!"
 elif test "$fpassword" != "$password"
 then
 echo "密码不正确!"
 else
 hour=`date +%H`
 if test "$hour" -gt 8
 then
 echo "你迟到了!"
 echo "$name 上班迟到---日期:`date`" >>/home/user/check.dat
 else
 echo "早上好,$name!"
 fi
 fi
 ;;
 2)echo "请输入你的名字:"
 read name 
 echo "请输入你的密码:"
 read password
 if test -r /home/user/userinfo.dat
 then
 while read fname fpassword
 do
 if test "$fname" = "$name"
 then
 break
 fi
 done < /home/user/userinfo.dat
 else
 echo System Error:userinfo.dat does not exist!
 fi
 if test "$fname" != "$name"
 then
 echo " 不存在该用户!"
 elif test "$fpassword" != "$password"
 then
 echo "密码不正确!"
 else
 hour=`date +%H`
 if test "$hour" -lt 18
 then
 echo "你早退了!"
 echo "$name 下班早退----日期:`date`">> /home/user/check.dat
 else
 echo "再见,$name!"
 fi
 fi
 ;;
 3)echo "请输入你的名字:"
 read name
 echo "请输入你的密码:"
 read password
 if test -r /home/user/userinfo.dat
 then
 while read fname fpassword
 do
 if test "$fname" = "$name"
 then
 break
 fi
done < /home/user/userinfo.dat 
else
 echo System Error:userinfo.dat does not exist!
 fi
 if test "$fname" != "$name"
 then
 echo "不存在该用户!"
 elif test "$fpassword" != "$password"
 then
 echo "密码不正确!"
 else
 echo "你的记录:"
 echo "---------"
 cat -b /home/user/check.dat|grep $name
 echo "---------"
 fi
 ;;
 4)echo "欢迎你的使用,再见!"
 exsig=1
 ;;
 *)echo "请输入合法的选项!"
 ;;
 esac
 if test "$exsig" = "1"
 then
 break
 fi
done
```


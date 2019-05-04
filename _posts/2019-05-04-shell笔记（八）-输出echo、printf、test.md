---
layout:     post
title:      shell笔记（八）
subtitle:   echo、printf、test
date:       2019-05-04
author:     Zhouyuxuan
header-img: img/post-bg-kuaidi.jpg
catalog: true
tags:
    - shell
    - 入门
---

### echo

```shell
echo string

#普通
echo "23333asd"
echo 23333asd

#转义
echo "\"2333\""
echo \"2333\"


#read命令：从标准输入中读取一行，并且把输入行的每个字段赋给变量
read name
echo "$name is a test"

#-e 开启转义
echo -e "2333 \n"		#换行
echo -e "2333 \c"		#不换行

#重定向
ehco "23333">2333.txt

#原样输出(利用'')
echo '$asd\"'
#输出$asd\"

#显示命令执行结果
echo `date`
#反引号，输出Thu Jul 24 10:12:23 CST 2019
```



## printf

模仿C程序库中的printf()，使用POSIX标准定义，使用printf比echo

```shell
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 
```

输出为

```shell
姓名     性别   体重kg
郭靖     男      66.12
杨过     男      48.65
郭芙     女      47.99
```

其他实例

```shell
# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样 
printf '%d %s\n' 1 "abc" 

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n" 
```

转义字符与C中的相同



## test命令：相当于条件判断
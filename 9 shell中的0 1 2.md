### shell中的0 1 2
shell中的0 1 2有着特殊的意义，分别为标准输入(STDIN)、标准输出(STDOUT)和标准错误(STDERR),在脚本中我们经常看到类似的写法`1>/dev/null 2>&1`它的意思是把标准输出和标准错误全部指向黑洞，不打印出来，程序完全在地下运行，像潜水艇一样悄无声息。
这3个数字都可以看出是文件描述符，对这些描述符经常有重定向的操作。
``` shell
n > filename 使用FD为n打开文件filename
n >> filename 追加模式打开文件filename
n < filename 读取模式打开文件filename
```
将文件重定向到标准输入、将标准输入重定向到文件常使用`exec`命令。
#### 将标准输入重定向到文件
文件hfile的值为`value1 value2`
``` shell
root:~# cat hfile 
value1
value2
```
``` shell
#!/bin/bash
exec 8<&0  #8也指向了键盘。
exec 0< hfile # 1. 使用标准输入打开文件。
read a        # 2. read将从stdin中读取。
read b
echo "---------------------------"
echo $a
echo $b
echo "Close FD 8:"
exec 0<&8 8<&-
echo -n "Pls. Enter Data: "
read c
echo $c
exit 0
```
执行结果为
``` shell
root:~# ./run.sh 
---------------------------
value1
value2
Close FD 8:
Pls. Enter Data: value3
value3
```
#### 将标准输出重定向到文件
``` shell
root:~# exec 4>log
root~# echo " I think befor I am" 1>&4
root:~# cat log
 I think befor I am
```

#### 知识点
* shell中的`0 1 2`为文件描述符，分别代表`标准输入、标准输出、标准错误`
* 常使用`exec`进行文件描述符的重定向。`exec 8<&0`表示8指向了键盘输入，`exec 9>&1`表示9指向了标准输出。

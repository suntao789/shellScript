### for循环实战
俗话说，光说不练假把式，学习任何一门学问，必须用起来才能记得住，当然shell脚本也是一样。今天分享下我遇到的一个问题，然后怎么用shell来实现的。
#### 问题描述
做一个项目的时候，需要把Linux下相关的进程全部重启一遍，重启的命令为`adm -cmd restartapp -app 进程名称`，并且必须使用ossadm用户。
进程名称和某一的方式目录文件夹名称相同，也就是说遍历该目录下的文件名称就可以得到进程的名称了。
那么应该如何写脚本那？
#### 问题分析
使用指定用户执行命令，可以使用`su ossadm -c "xxx"`的方式，当然前提是使用`root`用户登录。
遍历某一目录下的文件夹应该有很多方法了，我用到的是`ls | more | grep -v "Restart.sh"`，其中脚本的名称为`Restart.sh`，并且放到了该目录下，所以需要排除掉这个文件。
#### 脚本实例
实际的脚本非常简单
``` shell
#!/bin/bash
p=$(ls | more | grep -v "Restart.sh")
for i in $p
do
 echo $i
 echo $(su ossadm -c "adm -cmd restartapp -app $i")
done
```
一开始我把`$(su ossadm -c "adm -cmd restartapp -app $i")`写成了`su ossadm -c "adm -cmd restartapp -app $i"`，由于符合问题导致错误，所以特殊符合场景建议写成`$()`样式.
#### 知识点
* `grep -v`表示反转
* `$(su ossadm -c "xxx")`更改用户执行相关命令

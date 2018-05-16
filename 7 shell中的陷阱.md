### 好用的"陷阱"
脚本运行过程中，经常需要与操作人员进行交互，那就需要脚本在执行过程中监控键盘和鼠标的动作，当操作人员操作时，脚本可以实时捕获到这些信号，执行指定的操作。实现这个功能的就是shell中的`trap`。
先看一个例子：
``` shell
#!/bin/bash
task(){
 echo "You have pressed CTRL + C!"
}
trap task SIGINT
counter=0
while (:);do
 ((counter=$counter+1))
 echo "The date is : [ $(date) ]"
 sleep 1
 if [ "$counter" == 10 ] ; then
  exit 0
 fi
done
```
脚本里首先定义了一个函数`task`,`trap task SIGINT`表明用户在输入`CTRL+C`后会调用`task`函数来处理，`SIGINT`表示`CTRL+C`，类似的还有很多这样的定位，使用`trap -l`可以看到shell中所有定义的信号量。
代码`while (:)`等同于`while true`,为什么等同那，请大家思考下。
代码`echo "The date is : [ $(date) ]"` 等同于`echo "The date is : 'data'` ，之前样例中有类似的表述。
看下执行的结果
``` shell
test:~/shellScript # ./09.sh
The date is :  Tue May  8 10:44:29 GMT-8 2018
The date is :  Tue May  8 10:44:30 GMT-8 2018
The date is :  Tue May  8 10:44:31 GMT-8 2018
^CYou have pressed CTRL + C!
The date is :  Tue May  8 10:44:31 GMT-8 2018
The date is :  Tue May  8 10:44:32 GMT-8 2018
```
没有捕获`CTRL+C`信号量的时候，输入`CTRL+C`后代码会停止执行，捕获`CTRL+C`信号量后，程序没有退出而是打印了一行字符，请好好理解这个点。

#### 知识点
* `trap task SIGINT`捕获信号量`CTRL+C`并执行`task`函数
* `while (:)`等同于`while true`
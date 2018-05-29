### 开启tomcat服务
要开启tomcat的服务只需要调用`catalina.sh`就可以了，是一个很简单的事情。但在实际的项目中有很多可靠性的约束，把这些约束加到一起，就会发现要开启一个服务也是不简单的事情。
``` python
1、整个过程有日志记录，方便问题定位
2、启动前要检查文件是否可执行
3、启动后检查是否catalina的日志是否报错
```
#### 日志记录函数
之前已经多次涉及到这块，不再多说，代码如下
``` shell
function writelog
{
 nowtime=`date '+%Y-%-m-%d %H:%M:%S'`
 echo "[${nowtime}]$*" >> $LOG_FILE
}
```
#### 检查文件是否可执行
到`catalina.sh`所在的文件夹中，查看该文件是否可执行
``` shell
curr_path=`pwd`
PRGDIR=$curr_path
EXECUTABLE=catalina.sh
if [ ! -x "$PRGDIR"/"$EXECUTABLE" ]
then
 echo "Cannot find $PRGDIR/$EXECUTABLE"
 writelog "[fail] Start tomcat failed"
 exit 1
fi
```
#### 启动进程
找到相应文件启动，并记录启动日志
``` shell
writelog "Call $EXECUTABLE to start"
bash "$PRGDIR"/"$EXECUTABLE" start "$@"
```
#### 检查进程
进程启动完成后会在catalina.out中打印`Server startup`的字段，因此需要监控该日志
``` shell
COUNTER=0
until grep "Server startup" catalina.out
do
 if [ $COUNTER -gt 50 ]; then
  break
 fi
 COUNTER=`expr $COUNTER + 1`
 echo -n "."
 sleep 3
done
echo "done."
```
#### 启动完成后检查报错信息
进程启动完成后，有可能出现一些异常信息，因此需要搜索日志中的`error`和`exception`
``` shell
error=`grep -c "Error" catalina.out`
exception=`grep -c "Exception" catalina.out`
if [ ${error} -gt 0 -o ${exception} -gt 0 ]
then
 ${curr_path}/shutdown.sh >/dev/null 2>&1
 writelog "[fail] Startup failed."
 exit 1
else
 writelog "[success] Startup successfully."
 exit 0
fi
```
#### 知识点
* `expr $COUNTER + 1`表示数值运算，类似的还有`let $COUNTER + 1`,`$[ $COUNTER + 1 ]`,`$(($COUNTER+1))`
* `until grep XX; do XX done`又一种循环的方式
* `grep -c`用于计数
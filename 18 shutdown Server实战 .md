### shutdown Server实战
`纸上得来终觉浅 绝知此事要躬行。`其实学什么都是这个道理，实践是检验真理的唯一标准，前面已经把相关的基础大概都讲了一遍，今天我们实战一次。
需求描述
``` python
1、关闭某服务的相关进程，其包含进程有路径名称做标识
2、关闭服务和开启服务的用户要相同，或者用root用户关闭
3、有日志记录
```
#### 全局变量信息
全局变量有，当前用户、日志文件位置、程序文件位置等信息，示例如下
``` shell
old_path=`pwd`
prg_path=`dirname "$0"`
cd $prg_path
prg_path=`pwd`
install_path=`cd ${prg_path}/..;pwd`
cd $old_path
LOG_FILE="${prg_path}/../logs/Server.log"
curr_user=`id -un`
```
#### 日志记录模块
日志要记录时间信息，当前用户、和操作内容
``` shell
function writelog
{
 nowtime=`date '+%Y-%-m-%d %H:%M:%S'`
 echo "[${nowtime}][${curr_user}]$*" >> $LOG_FILE
}
```
#### 判断停止服务的用户和开启服务的用户是否相同
开启服务后，会生成一个`startupuser.cfg`的文件，文件的内容类似为`startup_user=root`。脚本要获取当前用户的名称以及去文件中读取开启用户的名称。代码示例为
``` shell
startup_user_key="startup_user"
startup_user_cfg_file="${prg_path}/startupuser.cfg"
pre_startup_user=""
if [ -f "${startup_user_cfg_file}" ];then
 pre_startup_user=`cat ${startup_user_cfg_file} | \
  awk -F= -v key="${startup_user_key}" '{if ( $1 == key ) print $2}'`
 if [ "X${curr_startup_user}" != "X${pre_startup_user}" \
   -a "X${curr_startup_user}" != "Xroot" ];then
   cd $old_pwd
   echo "*********************************"
   echo "Can't shutdown ${curr_startup_user}"
   echo "*********************************"
   writelog "[fail] Shutdown  failed."
   exit 1
 fi
fi
```
#### 找到并停止相关服务
要停止进程，需要先找到进程的pid，然后进行kill操作
``` shell
pids=`ps -ef|grep "java"|grep "$prg_path"|grep -v "grep"|awk '{print $2}'`
for pid in $pids
do
if [ "$pid" != "" ] ; then
 kill -9 $pid
 if [ $? -ne 0 ] ;then
  echo "********************"
  echo "Stop failed."
  echo "********************"
  writelog "[fail] Kill process $pid to stop  failed."
  exit 1
 fi
fi
done
```
#### 知识点
* `awk -v`表示将参数作为变量传递给awk处理语句
* `if [ XX -a XX ]`这里面的-a表示`and`，-o表示`or`
* `grep -v XX`表示将XX反选，即获取除XX之外的项
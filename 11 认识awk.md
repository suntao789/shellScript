### 认识awk
``` python
今天是汶川地震10周年，当前我在学校里，听同学们说四川地震了，电话都打不通，心中一惊，后来在网上看到灾区的惨状不禁泪流满面。一晃10年过去了，纪念汶川地震，也纪念我们的青春。想想地震中的人们，无论当前有什么困难，都可以鼓励我们砥砺前行。
```

awk是一个非常强大的工具，可以等同于一门语言，调用的方法有2种。
#### 命令行的方式
`awk [-F  field-separator]  'commands'  input-file(s)`
其中，commands 是真正awk命令，[-F域分隔符]是可选的。 input-file(s) 是待处理的文件。
我们看下`commands`命令的样例
``` shell
{ print $1 }
```
执行命令和结果如下
``` shell
root# awk -F : '{ print $1 }' /etc/passwd
bin
daemon
ftp
games
```
这行脚本的功能是打印所有用户的名称，也可以使用`cut`命令来实现
``` shell
#cut -f1 -d ":" /etc/passwd
bin
daemon
ftp
games
```
需要注意的是`awk`的分隔符是`-F`，而`cut`的分隔符为`-d`

#### 文件的方式
有时候`awk`处理的过程非常复杂，仅靠命令行书写起来不够方便，将`commands`写成文件的方式使用起来就比较方便了，而且看起来也比较整洁。
``` shell
BEGIN{
 print "This is the begin."	
}
{
 print "-------------------"
 print "Username : "$1
 print "Shell : "$6
}
END{
 print "This is the end."
}
```
把上述的`commands`保存为一个文件，使用`awk`调用，它输出的结果就显得高大上了。
``` shell
#awk -F : -f 12.awk /etc/passwd
This is the begin.
-------------------
Username : bin
Shell : /bin
-------------------
Username : daemon
Shell : /sbin
-------------------
Username : ftp
Shell : /srv/ftp
-------------------
Username : games
Shell : /var/games
```
做任何工作都需要包装，否则怎么体现出来能力那，后一种文件的方式输出结果要比命令行好看的多，虽然功能都是一样的。
#### 知识点
* `awk`的使用样式为`awk [-F  field-separator]  'commands'  input-file(s)`
* 针对`commands`有2种调用方法，直接输入命令和使用文件，使用文件灵活性更高。




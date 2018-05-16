### While 和 Until
在shell中，循环控制语句除了For外，还有While和Until，在一些场合使用这2个循环语句会更加方便。
#### While
下面我们来看看一段代码
``` shell
#!/bin/bash
password="pa55w0rd"
read -p "Password : " userinput
while [ ${password} != $userinput ]; do 
	echo "Login failed!"
	read -p "Password : " userinput
done
echo "Login ok!"
```
这里有个字符串比较，`[ ${password} != $userinput ]`时为`true`,类似的还有一种字符串比较的方法`test ${password} != $userinput`，这两种方法是等价的。

#### Until
Until语句用的较少，它的逻辑是当Until的判断为`true`时，退出循环。
``` shell
#!/bin/bash
password='pa55w0rd'
until [ "$userinput" == "$password" ] ; do
	read -p 'Please input your password : ' userinput
done
echo "Login ok!"
```
除退出循环的逻辑和while相反外，其他的语法都相同。

#### 知识点
* while的判断为false时，退出循环，Until正好与之相反。
* 判断语句`[ ${password} != $userinput ]`与`test ${password} != $userinput`等价

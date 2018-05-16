### 更改bash的分隔符
从前面的例子里我们知道，bash默认的分隔符为空白，包括空格、tab和新行，但有时候对文本操作的时候，更改shell默认的分隔符会更加的方便，可以使用`$IFS='XX'`的方式来修改。

**需求是：找到文件`passwd`中有`bash`执行权限的用户，去掉`:`并打印出来。**
``` shell
#!/bin/bash
echo "当前 bash 分隔符为 : $IFS"
IFS_OLD=$IFS
temp=$(cat /etc/passwd | grep '/bin/bash')
for user_line in $temp; do
	echo "--------------------------"
    echo $user_line
	IFS=':'
	for column in $user_line ; do
		echo $column		
	done
done
echo "--------------------------"

echo "输出完毕 , 此时分隔符为 : $IFS"
echo "正在恢复分隔符"
IFS=$IFS_OLD
echo "恢复完成 , 此时分隔符为 : $IFS"
echo "完毕"
```
由于新行是默认`IFS`的值，所以`for`循环可以打印所有的行。嵌套的`for`循环使用更改后的`IFS`打印每个字段。具体的打印的内容有
``` shell
当前 bash 分隔符为 :
--------------------------
root x 0 0 root /root /bin/bash
root
x
0
0
root
/root
/bin/bash
--------------------------
输出完毕 , 此时分隔符为 : :
正在恢复分隔符
恢复完成 , 此时分隔符为 :

完毕
```
#### 知识点
* **默认的`IFS`为空白，包括空格、tab、和换行**
* **`IFS`可以根据需要修改，可以带来意想不到的方便**

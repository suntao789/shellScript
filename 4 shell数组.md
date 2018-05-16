### 数组初探
shell作为一种编程语言，当然有数据结构相关的内容，比较典型的就是数组。数组有两种使用方式，一种是即用型的，比如`A=(1 2 3 4 5)`，另一种是先声明再使用的，比如`declare -A A; A[0]=1;A[1]=2`。
我们先看一个样例，主要功能是让用户选择要执行的脚本。
``` shell
#!/bin/bash
declare -A files # 声明一个关联数组
i=0
numbers=""
for filename in `ls -I $0 ./scripts/`
	do
		echo -e "${i}" '==>' "$filename"
		files[${i}]=${filename} # 使用关联数组建立用户选择的数字和脚本名称的对应关系
		numbers="${numbers}|${i}"
		i=$((i+1))
	done
numbers="("${numbers}"|)"
echo "请选择需要执行的脚本 : $numbers ?"
# 根据用户选择进行选择脚本执行
read userinput
for index in ${!files[@]};do
	if [ "$userinput" == $index ];then
		/bin/bash ./scripts/${files[$userinput]}		
		break
	fi
done
```
#### `${numbers}|${i}`
一开始，我把`|`看成了管道符，实际运行了下发现它其实就是一个字符，输出结果如下`|0`,代码`"("${numbers}"|)"`其实就起到了一个拼装的作用，最后输出的效果如下`(|0|1|2|3|4)`

#### `${!fies[@]}`
这里我们分开来看，符合`@`表示取出所有的元素，`file[@]`表示取出数组file的全部元素，`${files[@]}`表示对数组的元素取值，那么`${!files[@]}`表示什么意思那？其实它取出元素的下表，等同于`$(seq 0 ${#files[*]})`
### 知识点
* **数组创建的两种方式，`A=(1 2 3 4 5)`和`declare -A A; A[0]=1;A[1]=2`**
* **获取数组的所有元素有`${files[@]}`或者`${files[*]}`**
* **获取数组的长度`${#files[@]}`**
* **获取数组的下表`${!files[@]}`**


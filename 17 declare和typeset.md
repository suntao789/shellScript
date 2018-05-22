### declare和typeset
shell中`declare`和`typeset`都是内置命令，这两个命令用法完全相同，都用于声明命令，给变量设置属性值。主要的参数有
``` shell
“-f”：用于函数，显示函数定义。 
“-F”：用于函数，只显示函数名字，不显示函数定义。
“-i”：用于整数，可以进行数学运算。
“-l”：对变量赋值时，值的大写字母全部转换为小写。 
“-u”：对变量赋值时，值的小写字母全部转换为大写。 
“-r”：声明变量只读，不能被修改，也不能unset
```
耳听为虚，眼见为实，看个样例
``` shell
#!/bin/bash
func1 ()
{
  echo This is a function.
}

# 打印上面定义的函数
typeset -f  
      
# 打印空行
echo

# 定义var1为整数
typeset -i var1   
var1=2367
echo "var1 typesetd as $var1"

# 自增操作不用使用'let'
var1=var1+1
echo "var1 incremented by 1 is $var1."
# 将其改为小数会报错
echo "Attempting to change var1  2367.1."
var1=2367.1
echo "var1 is still $var1"

echo
# 设置变量为可读
typeset -r var2=13.36         
echo "var2 typesetd as $var2"

# 改变它的值会报错
var2=13.37 
echo "var2 is still $var2" 
```
执行结果为
``` shell
func1 ()
{
    echo This is a function.
}

var1 typesetd as 2367
var1 incremented by 1 is 2368.
Attempting to change var1 2367.1.
./14.sh: line 19: 2367.1: syntax error: invalid arithmetic operator (error token is ".1")
var1 is still 2368

var2 typesetd as 13.36
./14.sh: line 27: var2: readonly variable
var2 is still 13.36
```
#### 知识点
* `declare`和`typeset`用法相同，二者可以替换使用
* `-r`表示变量只读，`-i`表示为整数

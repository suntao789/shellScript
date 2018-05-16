### for循环
在看for循环之前，先看下一段简单的python代码
``` python
#!/usr/bin/env python
# encoding:utf-8

# 空格分隔
print "1 2 3 4 5"

# 逗号分隔
print "1,2,3,4,5"

# tab分隔
print "1\t2\t3\t4\t5"

# 回车(\r)分隔
print "1\r2\r3\r4\r5"

# 换行(\n)分隔
print "1\n2\n3\n4\n5"

# (\r\n)分隔
print "1\r\n2\r\n3\r\n4\r\n5"
```
相应的输出结果为
``` python
1 2 3 4 5
1,2,3,4,5
1	2	3	4	5
5
1
2
3
4
5
1
2
3
4
5
```
shell代码出场，代码为
``` shell
#!/bin/bash
for i in $(python ./hello.py)
	do 
		echo $i
	done
```
那么，shell代码的执行结果是什么那？
请看结果
``` shell
1
2
3
4
5
1,2,3,4,5
1
2
3
4
5
5
1
2
3
4
5
1
2
3
4
5
```
我们看到，对于空格、tab、换行符（\n,\r\n）等shell都识别为元素分隔的标识，只有逗号无法识别为分隔符。
### 知识点
* shell中的for循环格式为for i in XXX do XXX done
* 空格、tab、换行符均可识别为元素分隔标识，除逗号外




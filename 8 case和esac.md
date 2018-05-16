### case和esac
shell中的`case`语句使用的比较广泛，常用于提供诸多选项让用户去选择，比起`if`语句要简洁的多，我们看一个实例。
``` shell
#!/bin/bash
read -p "Please confirm (Y/N) : " userinput
echo "You have typed : $userinput"
case "$userinput" in
 Y|y|[Yy]|[Ee]|[Ss]) echo 'Your choosen is Y';;
 N|n|[Nn]|[Oo]) echo 'Your choosen is N';;
 *) 
  echo 'Input error!'
  echo '用两个分号结束是为了支持多条语句'
  ;;
esac
exit 0
```
使用`case`开始，`esac`结束，类比于`if`开始，`fi`结束，很有意思，我猜想几十年前开始`shell`的人一定非常有趣。
`Y|y|[Yy]|[Ee]|[Ss]`表示可以匹配`Y y E e S s`等字符
`*)`表示默认匹配，上面的case没有匹配上的，这里都会匹配。
`;;`2个分号表示语句执行完成后，`break`退出，当然也可以支持多条语句。

#### 知识点
* `case $i in XX ... esac`样式为case选择
* `case`分支支持多条语句，并以`;;`结束
*  `Y|y|[Yy]|[Ee]|[Ss])`为多条件匹配

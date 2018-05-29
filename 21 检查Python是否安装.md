### 检查Python是否安装
如何判断系统中是否已经安装Python？可以在linux中执行下`python -V`的命令，如果已经安装了，`$?`的值为0，并返回python的版本，否则`$?`的值为非0.
``` shell
function check_python_main
{
  typeset func_name="check_python_main"	
  typeset rule_script_path=`pwd`
  log_echo "Start to check python ..."
  python -V >/dev/null 2>/dev/null
  if [ $? -eq 0 ] ;then
     log_echo "has already installed python"
     return 0
  fi
  
  typeset python_file="python_3.2.3_linux.tar"
  download_software $1 ${python_file}
  if [ $? -ne 0 ] ;then  
  return 1
  fi
  
  create_python ${python_install_file} $2
  if [ $? -ne 0 ] ;then  
      return 1
  fi
  
  #test whether python is installed successful
  python -V >/dev/null
  if [ $? -eq 0 ] ;then
     log_echo "Install python successful..."
     return 0
  else
     log_echo "Failed to install the python..."
     return 1
  fi
}
check_python_main $@
if [ $? -ne 0 ];then
    return 1
fi
```
这段代码检查系统中有没有安装python，如果没有尝试下载安装，最后再次检查，每一个动作的后面都有是否执行成功的判断。
#### 知识点
* $?`表示上一个命令执行结果，成功返回值为0，失败为非0值
* `python -V >/dev/null 2>/dev/null`表示不打印任何信息
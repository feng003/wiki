> shell基础


    es="hello elasticsearch"
    echo ${es}
    unset es

    elk=("elasticsearch" "kibana" "logstash" "filebeat")
    #echo ${elk[2]}
    for i in ${elk[@]};
    do
    echo ${i};
    done

    #export JAVA_HOME=/usr/share/java
    #export JAVA_BIN=/usr/share/java/bin
    #export PATH=$PATH:$JAVA_HOME/bin
    #export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
    #export JAVA_HOME JAVA_BIN PATH CLASSPATH

    for file in /etc/*;
    do
    echo $file is file path \!;
    done

> 重定向


    一个进程默认会打开标准输入、标准输出、错误输出三个文件描述符
- [x]     输入重定向 < 


    read var < /tmp/log.txt
- [x]     输出重定向 > >> 2> &>


    echo 1234 > /tmp/log.txt
    
- [x]     输入和输出重定向组合使用


    cat > /tmp/file <<EOF
    I am $USER
    EOF

> 变量


    规则 字母 数字 下划线 不能以数字开头
    
##### 变量替换
- [ ]     变量名=变量值


    a=123
- [ ]     使用let为变量赋值


    let a=10+20
- [ ]     将命令赋值给变量


    l=ls
- [ ]     将命令结果赋值给变量，使用$() 或者 ``


    letc=$(ls -l /etc)
- [ ]     变量值有空格等特殊字符可以包含在 "" 或 '' 中

##### 变量的引用

    ${变量名} 称作对变量的引用
    echo ${变量名} 查看变量的值 
    ${变量名} 可以省略为 $变量名
    
##### 变量的作用范围

- [x]     变量的默认作用范围


    bash    
- [x]     变量的导出


    export 
- [x]     变量的删除


    unset 

> 环境变量、预定义变量与位置变量

- [x] 环境变量 每个shell打开都可以或得到的变量


    set env
    $? $$ $0
    $PATH
    $PS1
    
- [x] 位置变量


    $1 $2 ... $n
    
> 环境变量配置文件


    所有用户通用的
    /etc/profile
    /ect/profile.d/
    /etc/bashrc

    用户特有的
    ~/.bash_profile
    ~/.bashrc
    
> 数组 

- [x] 定义数组


    IPTS=(10 20 30 45)
- [x] 显示数组的所有元素


    echo ${IPTS[@]}
- [x] 显示数组元素个数


    echo ${#IPTS[@]}
- [x] 显示数组的第一个元素


    echo ${IPTS[0]}


[shell教程](http://manual.51yip.com/shell/)

[shell](https://edu.aliyun.com/lesson_155_1974?spm=5176.10731542.0.0.7ac11ac2ajvC6Y#_1974)

[Shell 编程入门到精通](https://edu.aliyun.com/course/155/lesson/list?utm_content=m_42087)
    
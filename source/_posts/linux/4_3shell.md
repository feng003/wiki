> if

- [x] 使用 if-then 语句


    if [ 测试条件成立 ] 或 命令返回值是否为0
    then 执行相应命令
    fi 结束
    if [ $UID=0]
    then echo $?
    fi
- [x] 使用if-then-else语句


    if [ 测试条件成立 ]; 或 命令返回值是否为0
    then 执行相应命令
    else 测试条件不成立 执行命令
    fi 结束
    if [$USER=root] ;then
        echo "root"
    else
        echo "other"
    fi

    if [测试条件成立];
    then 执行命令
    elif [测试条件成立];
    then 执行命令
    else 测试条件不成立 执行命令
    fi
    
- [x] 嵌套if的使用


    if [ 测试条件成立 ];
    then 执行命令
        if [ 测试条件成立 ];
        then 执行相应命令
        fi
    fi 结束
> case 和 select 构成分支


    case "$变量" in
        "情况1" )
            命令... ;;
        "情况2" )
            命令... ;;
        * )
            命令... ;;
    esac

    case "$1" in 
        "start" | "START")
        echo $0 "start " //cmd1
        ;;
        
        "stop")
        echo $0 "stop" 
        ;;
        
        "restart" | "reload")
        echo $0 restart
        ;;
        
        *)
        echo "Useage: $0 {start | stop | restart}"
    esac

> for


    for 参数 in 列表 {1..9}
    do 执行的命令
    done 封闭一个循环

    使用 反引号或 $() 执行命令，命令的结果当作列表进行处理
    for i in {1..9};
    do
        echo $i;
    done

    for filename in `ls *.pm3`
    do
        mv $filename $(basename $filename .mp3).mp4
    done

> C 语言风格的for命令


    for (( 变量初始化; 循环判断条件; 变量变化))
    do
        循环执行的命令
    done

    for (( i=1; i<=10; i++ ))
    do
        echo $i
    done

> while and until


    while test测试是否成立
    do
        命令
    done

    a=1
    while [$a -lt 10];
    do
        ((a++));
        echo $a;
    done

    until循环与while循环相反，循环测试为假时，执行循环，为真时循环停止
    
> break continue


    for sc_name in /etc/profile.d/*.sh
    do
        if [ -x $sc_name]; then
         . $sc_name
        fi
    done
    
> 使用循环处理命令行参数


    命令行参数可以使用 $1 $2 ${10} $n
    $0 代表脚本名称
    $*  $@ 代表所有位置参数
    $# 代表位置参数的数量

    for pos in $*
    do
        if [$pos = "help"]; then
            echo $pos
        fi
    done
    while [ $# -gt 1]
    do
        echo $# "do sth"
        shift //读取第二个
    done
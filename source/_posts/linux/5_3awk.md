> awk 根据定位到的 数据行 处理其中的分段

    任何awk语句都是有 模式 和 动作 组成

    分隔符默认是 空格； -F 参数指定字段分隔符
    $0 代表原来的行
    $1 代表第一个字段
    $N 代表第N个字段
    $NF 代表最后一个字段

    date | awk '{print "Y:"$6 "\tM:"$2 "\tD:"$3 }'
    awk '{print $0}' /var/local/logs/system_2020-02-11.log
    awk '{print $1}' /var/local/logs/system_2020-02-11.log
    awk '{print $1,$3}' /var/local/logs/system_2020-02-11.log

    BEGIN 语句设置计数和打印头部信息，在任何动作之前进行
    END   语句输出统计结果 在完成动作之后执行

    echo $PATH | awk 'BEGIN{RS=":"}{print $0}'
    echo $PATH | awk 'BEGIN{RS=":"}{print NR $0}'

    awk 'BEGIN {print "time content\n"} {print $1,$3} END{print "end of this"} ' /var/local/logs/system_2020-02-11.log > a.txt //开头 结尾 输入信息

    awk '{if ($1=="2020-02-10") print $0}' /var/local/logs/system_wms_2020-02-10.log

> awk 脚本的流程控制


    输入数据前例程 BEGIN{}
    主输入循环{}
    所有文件读取完成例程END{}
    
> awk的字段引用和分离


- [x] 每行称为AWK的记录


    使用空格、制表符分隔开的单词称作字段
    可以自己制定分隔的字段

- [x] 字段的引用


    awk中使用 $1 $2 $n 标识每一个字段
        awk '{priint $1,$2,$3}' filename
    awk 可以使用-F 选项改变字段分隔符
        awk -F ',' '{priint $1,$2,$3}' filename
        分隔符可以使用正则表达式
        
> awk表达式

- [x] 赋值操作符 = ++ -- += -= *= /= %= ^=


        var1="name"
        var2="hello" "world"
        var3=$1

- [x]     算数操作符 + - */ % ^

- [x]     系统变量


        FS OFS字段分隔符 OFS标识输出的字段分隔符
        RS 记录分隔符
        NR FNR 行数
        NF 字段数量 最后一个字段内容可以用 $NF取出

        head -5 /etc/passwd | awk -F ":" '{print $1}'
        head -5 /etc/passwd | awk 'BEGIN{FS=":"}{print $1,$2}'
        head -5 /etc/passwd | awk 'BEGIN{FS=":";OFS="-"}{print $1,$2}'
        
- [x]     关系操作符 > < <= >= == != ~(字符匹配) !~

- [x]     布尔操作符 && || ！
    
> awk判断和循环

- [x]     条件语句 


        if (表达式)
            awk 语句1
        [ else
            awk 语句2
        ]
        多个语句需要执行可以使用 {} 将多个语句包括起来
        awk '{if($2>80) print $1}' filename
        
- [x]     循环 while循环 do循环 for循环 break continue


        while(表达式)
            awk语句1

        do{
            awk 语句1
        }while(表达式)

        for (初始值; 循环判断条件; 累加)
            awk 语句1
        head -l filename | awk '{for(c=2;c<NF;c++) print $c}'
        head -l filename | awk '{for(c=2;c<NF;c++) sum+=$c; print $sum}'
        awk '{sum=0; for(c=2;c<NF;c++) sum+=$c; print $sum/(NF-1)}' filename

> awk 数组

- [x] 数组的定义


    数组名[下标]=值
    下标可以使用数字 也可以使用字符串
- [x] 数组的遍历


    for (变量 in 数组名)
        使用数组名[变量]的方式以此对每个数组的元素进行操作
        
    awk '{sum=0 for(col=2;col<=NF;col++) sum+=$col; ave[$1]=sum/(NF-1) } END { for(user in ave ) print user, ave[user]}' filename

    awk -f avg.awk filename 通过脚本处理文件
- [x] 删除数组


- [x] 命令行参数数组


    ARGC 参数的个数
    ARGV 每个参数的内容
    EBGIN{
        for (x=0;x<ARGC;x++)
            print ARGV[x]
        print ARGC
    }

> awk的函数

- [x] 算数函数


    sin() cos()
    int() 取整
    rand() srand()
    awk 'BEGIN{pi=3.14; print int(pi)}'
    awk 'BEIGN{srand(); print rand()}'

- [x] 字符串函数


    index(s, t)
    length(s)
    match(s, r) 字符串匹配
    split(s, a, sep) 字符串分隔
    gsub(r, s, t)  字符串替换
    sub(r, s, t)
    substr(s, p, n)
- [x] 自定义函数


    function 函数名 (参数) {
        awk 语句
        return awk变量
    }

[grep awk sed](https://zhuanlan.zhihu.com/p/55732705)
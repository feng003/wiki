##### sed stream editor 根据定位到的 数据行 修改数据

- [x]     模式空间


    将文件以行为单位 读取到内存
    使用sed的每个脚本对该行进行操作
    处理完成后输出该行

- [x]     sed的替换命令 s


    sed 's/old/new/' filename
    sed -e 'e/old/new' -e 's/old/new/' filename (替换多个)
    sed -i 's/old/new' 's/old/new' filename (写入)
        
    echo a a a > afile
    sed 's/a/aa/' afile
    sed 's!/!abc!' afile
    sed -e 's/a/aa' -e 's/aa/bb' afile
    sed 's/a/aa/;s/aa/bb/' afile

    sed -r 's/ab*/!' afile (0或多个)
    sed -r 's/ab+/!' afile (1或多个)
    sed -r 's/ab?/!' afile (1)
    sed -r 's/a|b/!' afile (a或b)
    sed -r 's/(aa)|(bb)/!' afile (aa或bb 分组)
    sed -r 's/(a.*b)/\1 \1/' (axyzb 两次输出)
    sed -r 's/(a.*b)/\1:\1/' cfile (axyzb:axyzb)

- [x]    替换命令加强版


    全局替换 s/old/new/g g global 全局替换
        s@old@new@g

    标志位  s/old/new/标志位
        数字 第几次出现才进行替换
        g   每次出现都进行替换
        p   打印模式空间的内容
            sed -n 'script' filename 阻止默认输出
        w   file将模式空间的内容写入到文件

    寻址 增加寻址后对匹配的行进行操作
        /正则表达式/s/old/new/g
        行号(数字 $)s/old/new/g
        可以使用两个寻址符号，也可以混合使用行号和正则地址

    head /etc/passwd | sed '2s/nologin/!/'
    head /etc/passwd | sed '/^bin/s/nologin/!/'
    head /etc/passwd | sed '/^bin/,$s/nologin/!/'

    分组 /regular/{ s/old/new/; s/old/new/ }

    sed脚本文件 sed -f sedscript filename -f加载脚本文件

- [x]     command


    s 查找

    d 删除(整行)
    a 在匹配后追加
    i 在匹配后插入
    c 替换

    r 读文件
    w 写文件

    n 下一行
    = 打印行号
    p 打印
    q 退出

    y 替换
    b
    t

    time sed -n '10q' /etc/passwd
    time sed -n '1,10p' /etc/passwd
    sed -n '2p' /etc/passwd
    sed -n '1,4p' /etc/passwd
    sed -n '1,4!p' /etc/passwd
    sed -n '4,+4p' /etc/passwd

    sed '2i#####' /etc/passwd > a.txt 插入
    sed '$a#####' /etc/passwd > a.txt 追加
    sed '3c#####' /etc/passwd > a.txt 替换
    sed '2,4H;$G' /etc/passwd > a.txt 复制粘贴

    sed '/^$/d' /etc/passwd > a.txt 删除空行

    sed '/ext4/w newfastab' /etc/fstab 查找 ext4 另存为
- [x]   多行模式


    N 将下一行加入到模式空间
    D 删除模式空间中的第一个字符到第一个换行符
    P 打印模式空间中的第一个字符到第一个换行符
    T

    sed 'N;s/hello/!!!/' a.txt
    
- [x] 保持空间命令


    h/H 复制
    g/G 粘贴
    x 交换模式空间和保持空间
    cat -n /ect/passwd | head -6 | sed -n '1!G;h;$p'
    cat -n /ect/passwd | head -6 | sed -n '1!G;h;$!d'

- [x]     option


    **-n 抑制自动输出 读取下一个输入行**
    -e 执行多个sed指令
    -f 运行脚本
    **-i 编辑文件内容**
    -i.bak 编辑的同时创造.bak的备份
    **-r 使用扩展的正则表达式**
    
[grep awk sed](https://zhuanlan.zhihu.com/p/55732705)
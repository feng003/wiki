> 正则表达式与文本搜索

- [x] 元字符


    . 匹配除换行符外的任意单个字符
    * 匹配任意一个跟在它前面的字符
    [] 匹配方括号中的字符类中的任意一个
    ^ 匹配开头
    $ 匹配结尾
    \ 转义后面的特殊字符

    grep echo  /root/elasticsearch.sh
    grep pass.... /root/anaconda-ks.cfg
    grep echo.*  /root/elasticsearch.sh
    grep ^#  /root/elasticsearch.sh
    grep \.  /root/elasticsearch.sh
    grep "\."  /root/elasticsearch.sh
    
- [x] 扩展元字符


    + 匹配前面的正则表达式 至少出现一次
    ? 匹配前面的正则表达式 出现零次或一次
    | 匹配它前面或后面的正则表达式

- [x] find 查找


    find 路径 查找条件 [补充条件]
    find / -name nginx*
    find /etc -regex .etc.*wd$
    find /etc -type f -regex .*conf
    find /etc/ -atime 8  ( -user root  -uid 0 )
    find *.txt -exec rm -v {} \;
    cut -d ":" -f7 /etc/passwd | uniq -c
    cut -d ":" -f7 /etc/passwd | sort | uniq -c

- [x] grep  global search regular expression and print out the line


    基于正则表达式查找 满足条件的 行

    grep pattern file
    ^ 开头
    $ 结尾
    * 0到无穷个前一个字符
    [list] 字符集合 列出想要选择的字符
    [n-n]

    grep -n pattern file 显示行号
    grep -i pattern file 忽略大小写
    grep -v pattern file 不显示匹配的行
    grep -o pattern file 把每个匹配的内容用独立的行 显示
    grep -E pattern file 使用扩展正则表达式
    grep -A -B -C pattern file 打印命中数据的上下文
    grep pattern -r dir/ 递归搜索
    --color

    ps -ef | grep bash
    ps -ef | grep bash | grep -v grep
    echo "ABC" | grep -i ab
    echo "1234 7654" | grep -o "[0-9]4"
    echo "1234 7654" | grep -oE "[0-9]4|76"
    grep g[ao]  // ga go
    grep ^[^#]  // 搜索不以 # 开头的行
    grep [0-9]
    grep ^[a-z]
    grep ^[^a-zA-Z]

    grep ^$ /etc/passwd


[grep awk sed](https://zhuanlan.zhihu.com/p/55732705)
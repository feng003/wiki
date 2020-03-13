> man manual  help  info

    man ls
    man 7 ls
    man -a ls

    help cd
    ls --help

    info ls
    
> file 

- [x]  ls [OPTION]... [FILE]...


    ls -ll (l 长格式显示文件)
    ls-lah (a 显示隐藏 h print sizes in human readable format)  
    ls -lrt (r 逆序 t时间顺序)

    du -sh
    du -h –max-depth=1 *

- [x]  cd  Change the shell working directory


    help cd
    cd -
    cd ..

    pwd

- [x]  新建 删除文件夹mkdir / rmdir


    mkdir a b c 创建多个目录
    mkdir -p /a/b/c/d 建立多级目录.

    rmdir -p /a/b/c/d

- [x]  复制移动操作 cp / mv / rname   


    匹配符  * 所有 ? 单个字符
    cp -v -a -p -r
    mv -r

- [x]  文本查看 cat head tail wc


    cat 
    head -5 
    tail -f 文件内容更新后 显示信息同步更新
    wc 统计文件内容信息

- [x]  打包压缩和解压 tar gzip bzip2


    打包  c打包 x解包  f 指定类型为文件
    tar czf /home/ubuntu/etc-bak.tar /etc
    tar czf /home/ubuntu/etc-bak.tar.gz /etc
    tar cjf /home/ubuntu/etc-bak.tar.bz2 /etc
    
- [x]  vi


    正常模式
    hjkl
    复制 yy p
    剪切 dd p
    撤销 u
    替换 r
    移动到某行  g/G  2gg  10gg
    移动行头 shift+^   行尾shift+$

    命令模式
    :wq
    :w filename
    :!ifconfig
    搜索  /search
    替换  :s/x/X 当前行替换x为X  :%s/x/X/g 当前文件替换
    vim /etc/vim/vimrc 添加 set nu

    可视模式
    v
    V
    Ctrl+v 块可视模式
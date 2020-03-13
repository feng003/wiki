> 函数

- [x] 自定义函数


    function fname(){
        命令
    }

    fname
    函数作用范围的变量
        local 变量名
    函数的参数
        $1 $2 $3 $n
    cdls(){
        cd $1
        ls
    }

    checkpid 1 2 3 
    checkpid (){
        local i
        for i in $* ;
        do
            [ -d "/proc/$i" ] && return 0
        done
        return 1    
    }

- [x] 系统函数


    系统自建了函数库，可以在脚本中应用
    /etc/init.d/functions

    自建函数库 使用 source 函数脚本文件 导入函数
    /etc/profile
    .bashrc
    
> 脚本控制

- [x] 脚本优先级控制


    使用nice 和 renice 调整脚本优先级
    避免出现 不可控的 死循环

    ulimit -a
    core file size          (blocks, -c) 0
    data seg size           (kbytes, -d) unlimited
    scheduling priority             (-e) 0
    file size               (blocks, -f) unlimited
    pending signals                 (-i) 15234
    max locked memory       (kbytes, -l) 64
    max memory size         (kbytes, -m) unlimited
    open files                      (-n) 65535
    pipe size            (512 bytes, -p) 8
    POSIX message queues     (bytes, -q) 819200
    real-time priority              (-r) 0
    stack size              (kbytes, -s) 10240
    cpu time               (seconds, -t) unlimited
    max user processes              (-u) 4096
    virtual memory          (kbytes, -v) unlimited
    file locks                      (-x) unlimited
    func() { func | func& }
    func
    .(){. | .&}; 执行.  fork炸弹
    
- [x] 捕获信号


    kill 默认会发送 15号 信号给应用程序
    ctrl+c 发送 2号 信号给应用程序
    9号 信号 不可阻塞

    捕获15号信号 2号信号
    trap "echo signle 15" 15
    trap 'echo signle 2' 2
    echo $$
    while :
    do
        :
    done

> 计划任务

- [x]     一次性计划任务 at


    at 18:30
    at> echo hello > /tmp/hello.txt
    at> <EOT

- [x]     周期性计划任务 cron


    crontab -e
    crontab -l
    配置格式：
    分钟 小时 日期 月份 星期 执行的命令
    * * * * * /usr/bin/date >> /tmp/date.txt
    30 3 * * 1 /usr/bin/date >> /tmp/date.txt
    查看 cron 日志 tail -f cron
    ls /var/spool/cron

- [x]     计划任务加锁flock


    cd /etc/cron.d/
    vim /etc/anacrontab
    cat /etc/cron.daily/logrotate
    anacontab 延时计划任务
    flock 锁文件
    flock -xn "/tmp/f.lock" -c "/root/a.sh"
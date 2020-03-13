> 内存与磁盘管理


    top
    top - 10:42:52 up 13 days, 17:55,  3 users,  load average: 0.05, 0.05, 0.01 // 系统运行时间和平均负载

    Tasks: 126 total,   1 running, 125 sleeping,   0 stopped,   0 zombie // 任务
    Cpu(s):  2.6%us(user),  0.7%sy(system),  0.0%ni(niced), 96.6%id,  0.0%wa(IO wait),  0.0%hi,  0.0%si,  0.0%st // CPU 状态
    Mem:   8061376k total,  6911844k used,  1149532k free,   331912k buffers //内存使用
    Swap:        0k total,        0k used,        0k free,  2829292k cached
    

    进程ID-用户名-调度优先级-nice值-虚拟内存-驻留内存-共享内存-进程的状态
    PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND                              
    11796 apache    20   0  377m  21m 6268 S  2.3  0.3   0:41.02 httpd                                 
    2007 mysql     20   0 1785m 603m 5600 S  0.3  7.7  28:47.16 mysqld                                
    13766 root      20   0 4597m 1.1g  19m S  0.3 13.8  13:22.45 java                                  
    17471 root      20   0 15024 1316  996 R  0.3  0.0   0:00.02 top


[30个实例详解TOP命令](https://linux.cn/article-2352-1.html)

##### 内存和磁盘使用率查看

- [x] 内存使用率查看


    free / free -m
    total       used       free     shared    buffers     cached
    Mem:       3924416    3605520     318896    2512       226796    1538252
    -/+ buffers/cache:    1840472    2083944
    Swap:            0          0          0

    top
    Mem:   3924416k total,  3607100k used,   317316k free,   226876k buffers
    Swap:        0k total,        0k used,        0k free,  1539776k cached

- [x] 硬盘使用率查看


    fdisk -l 查看

    df -h
    Filesystem      Size  Used Avail Use% Mounted on
    udev            413M     0  413M   0% /dev
    tmpfs            87M  9.3M   78M  11% /run
    /dev/vda1        50G   18G   30G  38% /
    tmpfs           433M   48K  433M   1% /dev/shm
    tmpfs           5.0M     0  5.0M   0% /run/lock
    tmpfs           433M     0  433M   0% /sys/fs/cgroup
    cgmfs           100K     0  100K   0% /run/cgmanager/fs
    tmpfs            87M     0   87M   0% /run/user/106
    tmpfs            87M     0   87M   0% /run/user/500

    ls -lh /ect/passwd
    du /etc/passwd 实际占有空间

##### ext4文件系统

- [x]     ext4文件系统基本结构


    超级块

    超级块副本

    i节点 inode   ls -i
        vim 操作文件会修改inode
        echo > 操作不会
        ln afile bfile inode节点不变
        ln -s afile bfile 软链接
        getfacl afile
        setfacl -m u:user1:r 文件设置用户权限

    数据库 datablock
    
- [x]     xfs
- [x]     ntfs

##### 磁盘的分区与挂载 /etc/fstab

- [x]     fdisk


    fdisk -l

    Disk /dev/vda: 107.4 GB, 107374182400 bytes
    255 heads, 63 sectors/track, 13054 cylinders
    Units = cylinders of 16065 * 512 = 8225280 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0x0003a7b4

       Device Boot      Start         End      Blocks   Id  System
    /dev/vda1   *           1       13055   104855552   83  Linux

    Disk /dev/vdb: 107.4 GB, 107374182400 bytes
    16 heads, 63 sectors/track, 208050 cylinders
    Units = cylinders of 1008 * 512 = 516096 bytes
    Sector size (logical/physical): 512 bytes / 512 bytes
    I/O size (minimum/optimal): 512 bytes / 512 bytes
    Disk identifier: 0xa9cf4493

       Device Boot      Start         End      Blocks   Id  System

    fdisk /dev/sda1
    m help
    n new partition
    p primary e extended
    +50G
    p 查看分区情况
    d delete
    w save q quit

- [x]     mkfs


    mkfs.ext4 /dev/sda1

- [x]     mount 不可以操作磁盘，磁盘挂载到目录后，可以操作目录 


    /dev/vda1 on / type ext4 (rw)
    proc on /proc type proc (rw)
    sysfs on /sys type sysfs (rw)
    devpts on /dev/pts type devpts (rw,gid=5,mode=620)
    tmpfs on /dev/shm type tmpfs (rw)
    /dev/vdb1 on /mnt type ext3 (rw)
    none on /proc/sys/fs/binfmt_misc type binfmt_misc (rw)

    mkdir /mnt/sdc1
    mount /dev/sdc1 /mnt/sdc1

- [x]     parted

##### 磁盘配额的使用 quota

    mkfs.xfx /dev/sdb1

    mkdir /mnt/disk1
    mount -o uquota,gquota /dev/sdb1 /mnt/disk1

    chmod 1777 /mnt/disk1
    xfs_quota -x -c 'report -ugibh' /mnt/disk1
    xfs_quota -x -c 'limit -u isoft=5 ihard=10 user1' /mnt/disk1

##### 交换分区的查看与创建

- [x] 增加交换分区的大小


    ls /dev/sdd1
    mkswap /dev/sdd1 设置
    swapon /dev/sdd1 打开
    swapoff /dev/sdd1 关闭
    free -m

- [x] 使用文件制作交换分区


    dd if=/dev/zero bs=4M count=1024 of=/swapfile
    mksawp /swapfile
    ls -l /swapfile
    chmod 600 /swapfile
    
##### 软件RAID的使用

    raid 0  2块磁盘 striping 条带方式 提高单盘吞吐率
    raid 1  2块磁盘 mirroring 镜像方式 提高可靠性
    raid 5  3块磁盘 奇偶校验
    raid 10 4块磁盘 raid1 和 raid0的结合

    mdadm -C /dev/md0  -a yes -l1 (raid1) -n2 /dev/sd[b,c]1 创建磁盘阵列
    echo DEVICE /dev/sd[b,c]1
    echo DEVICE /dev/sd[b,c]1 >> /etc/mdadm.conf
    mdadm -Evs >> /etc/mdadm.conf
    mdadm --stop /dev/md0 停止raid
    dd if=/dev/zero of=/dev/sdb1 bs=1M count=1
    mkfs.xfs /dev/md0 格式化raid 设备

##### 逻辑卷管理

- [x] 逻辑卷 和 文件系统关系

- [x] 为linux创建逻辑卷


    pvcreate /dev/sd[b,c,d]1 创建vg1
    pvs
    vgcreate vg1 /dev/sdb1 /dev/sdb2 加入卷组
    lvcreate -L 100M -n lv1 vg1
    lvs 显示

    mkdir /mnt/test
    mkfs.xfs /dev/vg1/lv1
    mount 
    
- [x] 动态扩容逻辑卷


    vgextend centos /dev/sdd1
    lvextend -L +50G /dev/centos/root 
    df -h
    xfs_growfs /dev/centos/root

##### 系统综合状态查看

- [x]     sar


    sar -u 1 10 查看cpu 每隔一秒
    sar -r 1 10 查看内存
    sar -b 1 10 查看io
    sar -d 1 10 查看磁盘
    sar -q 1 10 查看进程
    
- [x]     第三方命令


    iftop -P 查看网络
    
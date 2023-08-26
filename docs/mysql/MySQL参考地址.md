> 插入数据


    CREATE TABLE `test` ( 
      `id` int(11) NOT NULL, 
      `a` int(11) DEFAULT NULL, 
      `b` int(11) DEFAULT NULL, 
      `c` int(11) DEFAULT NULL, 
      PRIMARY KEY (`id`) 
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

    DROP PROCEDURE IF EXISTS test_initData; 
    DELIMITER $ 
    CREATE PROCEDURE test_initData() 
    BEGIN 
        DECLARE i INT DEFAULT 1; 
        WHILE i<=100000 DO 
            INSERT INTO test(id,a,b,c) VALUES(i,i*2,i*3,i*4); 
            SET i = i+1; 
        END WHILE; 
    END $ 
    CALL test_initData(); 


[数据库索引原理及优化](https://www.cnblogs.com/chihirotan/p/7486035.html)

[数据库索引，到底是什么做的？](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961486&idx=1&sn=b319a87f87797d5d662ab4715666657f&chksm=bd2d0d528a5a84446fb88da7590e6d4e5ad06cfebb5cb57a83cf75056007ba29515c85b9a24c&scene=21#wechat_redirect)
    
    
[详细分析MySQL事务日志(redo log和undo log)](https://www.cnblogs.com/f-ck-need-u/p/9010872.html)

[两万字的数据库面试题，不看绝对后悔](https://mp.weixin.qq.com/s/lBnAmpYRmBylaRt9hIq4sw)

[24 个必须掌握的数据库面试问题！](https://mp.weixin.qq.com/s?__biz=MzU0OTk3ODQ3Ng==&mid=2247485460&idx=1&sn=3543e2316b811604333b2d4bbda57948&chksm=fba6e017ccd16901681a2bdd3021f7f40c54820187570d60aebe94f6211075ea6e4d99df0ba0&scene=27#wechat_redirect)

[mysql binlog应用场景与原理深度剖析](https://mp.weixin.qq.com/s/-CXTVPkUdMkT-6PB3lHLRw)

[关于MySQL，你未必知道的！](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651961957&idx=1&sn=c4cdf7c27ade9c95fdf40c4c38e19da9&chksm=bd2d0fb98a5a86af13ec7f096bde37e1c8cd0d19e7124e6bdb53761314d5b64a39ba9fbd1355&mpshare=1&scene=1&srcid=0425YGitxDXVLymMnLGHAzQq##)

[MySQL的索引是什么？怎么优化？](https://my.oschina.net/liughDevelop/blog/1788148)

[关于 InnoDB  锁 的超全总结](https://www.cnblogs.com/michael9/p/12443975.html)
---
title: "1_3SQL子查询"
date: 2020-03-11
tag: "子查询" 
---

> 子查询 嵌套在查询中的查询 查询结果集

##### 关联子查询 非关联子查询

    关联子查询：如果子查询的执行依赖于外部查询，通常情况下都是因为子查询中的表用到了外部的表，并进行了条件关联，因此每执行一次外部查询，子查询都要重新计算一次。

    非关联子查询：子查询从数据表中查询了数据结果，如果这个数据结果只执行一次，然后这个数据结果作为主查询的条件进行执行。

##### 存在性检测子查询 (exist子查询) 集合比较子查询(in some any all)


    exist子查询
    IN 
    ANY
    ALL
    SOME

    SELECT * FROM A WHERE cc IN (SELECT cc FROM B)
    SELECT * FROM A WHERE EXIST (SELECT cc FROM B WHERE B.cc=A.cc)
    //在cc建立索引的情况下， 如果A表比B表大 in 要比 exist子查询效率要高
    
---

    索引是个前提，其实和选择与否 还是要看表的大小。选择的标准，你可以理解为： 小表驱动大表。这种方式下效率是最高的。比如
    SELECT * FROM A WHERE cc IN (SELECT cc FROM B)
    SELECT * FROM A WHERE EXIST (SELECT cc FROM B WHERE B.cc=A.cc)
    当A小于B时，用EXIST。因为EXIST的实现，相当于外表循环，实现的逻辑类似于：
        for i in A
            for j in B
                if j.cc == i.cc then ...

    当B小于A时，用IN，因为实现的逻辑类似于：
        for i in B
            for j in A
                if j.cc == i.cc then ...
    所以哪个表小就用哪个表来驱动，A表小 就用EXIST，B表小 就用IN

---

    
##### 使用子查询作为计算字段出现在select查询中


    SELECT team_name, (SELECT count(*) FROM player WHERE player.team_id = team.team_id) AS player_num FROM team
    
> SQL 标准 连接表

##### SQL标准 SQL92 和 SQL99

##### SQL92中的5种连接方式

- [x] 笛卡尔积 cross join


    //两张表的乘积
    SELECT * FROM player, team

- [x] 等值连接 两张表中都存在的列进行连接



    SELECT player_id, player.team_id, player_name, height, team_name FROM player, team WHERE player.team_id = team.team_id
    
- [x] 非等值连接


    如果连接多个表的条件是等号时，就是等值连接，其他的运算符连接就是非等值查询。
    SELECT p.player_name, p.height, h.height_level FROM player AS p, height_grades AS h WHERE p.height BETWEEN h.height_lowest AND h.height_highest

- [x] 外连接(左连接 右连接)


    //SQL99标准
    SELECT * FROM player LEFT JOIN team on player.team_id = team.team_id

    SELECT * FROM player RIGHT JOIN team on player.team_id = team.team_id

- [x] 自连接


    //比布雷克·格里芬高的球员
    SELECT b.player_name, b.height FROM player as a , player as b WHERE a.player_name = '布雷克-格里芬' and a.height < b.height
    
> SQL99连接

##### SQL99标准中的连接查询

- [x] 交叉连接(笛卡尔积) CROSS JOIN


    SELECT * FROM player CROSS JOIN team
    SELECT * FROM t1 CROSS JOIN t2 CROSS JOIN t3

- [x] 自然连接(等值连接)


    //NATURAL JOIN
    SELECT player_id, team_id, player_name, height, team_name FROM player NATURAL JOIN team 
- [x] on连接


    SELECT p.player_name, p.height, h.height_levelFROM player as p JOIN height_grades as h ON height BETWEEN h.height_lowest AND h.height_highest

- [x] using连接


    SELECT player_id, team_id, player_name, height, team_name FROM player JOIN team USING(team_id)

- [x] 外连接


    LEFT JOIN
    RIGHT JOIN
    FULL JOIN
    
- [x] 自连接


    SELECT b.player_name, b.height FROM player as a JOIN player as b ON a.player_name = '布雷克-格里芬' and a.height < b.height

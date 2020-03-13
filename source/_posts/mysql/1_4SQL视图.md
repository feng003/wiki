---
title: "1_4SQL视图"
date: 2020-03-11
tag: "视图" 
---

> 视图

##### 视图 创建、更新和删除

- [x] 创建 CREATE VIEW


    CREATE VIEW view_name as 
    SELECT column1,column2 FROM table WHERE condition

    CREATE VIEW player_above_avg_height AS 
    SELECT player_id, height 
    FROM player
    WHERE height > (SELECT AVG(height) from player)

    SELECT * FROM player_above_avg_height
- [x] 嵌套视图


    CREATE VIEW player_above_above_avg_height AS
    SELECT player_id, height
    FROM player
    WHERE height > (SELECT AVG(height) from player_above_avg_height)

- [x] 修改视图 ALTER VIEW


    ALTER VIEW view_name as 
    SELECT column1, column2
    FROM table
    WHERE condition

    ALTER VIEW player_above_avg_height AS
    SELECT player_id, player_name, height
    FROM player
    WHERE height > (SELECT AVG(height) from player)

- [x] 删除视图 DELETE VIEW  


    DROP VIEW view_name
    DROP VIEW player_above_avg_height

##### 使用视图来简化SQL操作

- [x] 利用视图完成复杂的连接

- [x] 利用视图对数据进行格式化


    CREATE VIEW player_team AS 
    SELECT CONCAT(player_name, '(' , team.team_name , ')') AS player_team FROM player JOIN team WHERE player.team_id = team.team_id
    
- [x] 使用视图与计算字段


    CREATE VIEW game_player_score AS
    SELECT game_id, player_id, (shoot_hits-shoot_3_hits)*2 AS shoot_2_points, shoot_3_hits*3 AS shoot_3_points, shoot_p_hits AS shoot_p_points, score  FROM player_score

##### 视图和临时表

> 存储过程 是程序化的SQL

##### 创建一个存储过程 由sql语句和流程控制语句共同组成

    CREATE PROCEDURE 存储过程名称([参数列表])
    BEIGN
        需要执行的语句
    END

    DROP PROCEDURE
    ALTER PROCEDURE

    CREATE PROCEDURE `add_num`(IN n INT)
    BEGIN
        DECLARE i INT; 
        DECLARE sum INT; 
        SET i = 1; 
        SET sum = 0; 
        WHILE i <= n DO 
            SET sum = sum + i;
            SET i = i +1;
        END WHILE;
        SELECT sum;
    END

##### 流控制语句

- [x]     IF... THEN ... ENDIF
- [x]     CASE


    CASE
        WHEN expression1 THEN ... 
        WHEN expression2 THEN ... ... 
        ELSE
        --ELSE语句可以加，也可以不加。加的话代表的所有条件都不满足时采用的方式。
    END
- [x]     LOOP LEAVE ITERATE
- [x]     REPEAT UNTIL END REPEAT
- [x]     WHILE DO END WHILE
    
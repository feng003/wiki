---
title: "index"
date: 2020-03-12
---

[mysqldump](./mysqldump.md)

    select score, ROW_NUMBER() OVER(order by score desc) AS 'rank' from Scores;

    SELECT S.score, DENSE_RANK() OVER (PARTITION BY id ORDER BY S.score DESC ) AS 'rank' FROM Scores S;

[leetcode](./leetcode.md)

    181. 超过经理收入的员工
    184. 部门工资最高的员工
    197. 上升的温度
    1934.确认率

[高性能MySQL](./高性能MySQL.md)
---
title: "index"
date: 2020-03-12
---

[mysqldump](./mysqldump.md)

    select score, ROW_NUMBER() OVER(order by score desc) AS 'rank' from Scores;

    SELECT S.score, DENSE_RANK() OVER (PARTITION BY id ORDER BY S.score DESC ) AS 'rank' FROM Scores S;

[leetcode](./leetcode.md)
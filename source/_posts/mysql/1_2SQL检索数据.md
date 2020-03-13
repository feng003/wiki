---
title: "1_2SQL检索数据"
date: 2020-03-03
tag: "检索数据" 
---

> 检索数据

##### select 查询的基础语法

    查询列
    起别名 as
    查询常数
    去除重复行 distinct
    如何排序检索数据 order by
    约束返回结果的数量 limit

##### select 的执行顺序

    关键词的顺序是不能颠倒的
    select ... from ... where ... group by ... having ... order by ...
    select 语句的执行顺序
    from > where > group by > having > select的字段 > distinct > order by > limit 
    

    SELECT DISTINCT player_id, player_name, count(*) as num #顺序5

    FROM player JOIN team ON player.team_id = team.team_id #顺序1

    WHERE height > 1.80 #顺序2

    GROUP BY player.team_id #顺序3

    HAVING num > 2 #顺序4

    ORDER BY num DESC #顺序6

    LIMIT 2 #顺序7
    

---

> 数据过滤

##### 使用比较运算符对字段的数值进行比较筛选

    =  != < <= > >= between  is null

##### 使用逻辑运算符 

    and or in not

##### 使用通配符对数据条件进行复杂过滤

    like % 代表零个或多个字符 _ 代表一个字符
    使用 like "%str%" 或 like "%str" 会对全表进行扫描
    使用 like "str%" 会进行索引
    

---

    在MySQL中，支持两种排序方式：FileSort和Index排序。Index排序的效率更高，
    Index排序：索引可以保证数据的有序性，因此不需要再进行排序。
    FileSort排序：一般在内存中进行排序，占用CPU较多。如果待排结果较大，会产生临时文件I/O到磁盘进行排序，效率较低。

    所以使用ORDER BY子句时，应该尽量使用Index排序，避免使用FileSort排序。
    当然具体优化器是否采用索引进行排序，你可以使用explain来进行执行计划的查看。
    优化建议：
    1、SQL中，可以在WHERE子句和ORDER BY子句中使用索引，目的是在WHERE子句中避免全表扫描，ORDER BY子句避免使用FileSort排序。
    当然，某些情况下全表扫描，或者FileSort排序不一定比索引慢。但总的来说，我们还是要避免，以提高查询效率。
    一般情况下，优化器会帮我们进行更好的选择，当然我们也需要建立合理的索引。
    2、尽量Using Index完成ORDER BY排序。
    如果WHERE和ORDER BY相同列就使用单索引列；如果不同使用联合索引。
    3、无法Using Index时，对FileSort方式进行调优。

---

    如果你使用了WHERE子句，对于某个字段进行了条件筛选，那么这个字段你可以通过建立索引的方式进行SQL优化。
    因为我们在进行SQL优化的时候，应该尽量避免全表扫描。所以当我们使用WHERE子句对某个字段进行了条件筛选时，如果我们没有对这个字段建立索引，就会进入到全表扫描，因此可以考虑对这个字段建立索引。

    当然你也需要注意 索引是否会失效。因此除了考虑建立字段索引以外，你还需要考虑：
    1、不要在WHERE子句后面对字段做函数处理，同时也避免对索引字段进行数据类型转换
    2、避免在索引字段上使用<>，!=，以及对字段进行NULL判断（包括 IS NULL, IS NOT NULL）
    3、在索引字段后，慎用IN和NOT IN，如果是连续的数值，可以考虑用BETWEEN进行替换
    因为在WHERE子句中，如果对索引字段进行了函数处理，或者使用了<>,!=或NULL判断等，都会造成索引失效。

---

> SQL 函数

##### 常用函数

    算术函数    ABS() MOD() ROUND()
    字符串函数  
        CONCAT()    拼接
        LENGTH()
        CHAR_LENGTH()
        LOWER()
        UPPER()
        REPLACE()
        SUBSTRING()
    日期函数
        CURRENT_DATE()
        CURRENT_TIME()
        CURRENT_TIMESTAMP()
        EXTRACT()
        DATE()
        YEAR()
        MONTH()
        DAY()
        HOUR()
        MINUTE()
        SECOND()
    转换函数 CAST() COALESCE()
    
##### 聚集函数

    COUNT()
    MAX()
    MIN()
    SUM()
    AVG

##### 数据进行分组，并进行聚集统计 GROUP BY

##### 使用HAVING 过滤分组


---

    ORDER BY就是对记录进行排序。如果你在前面用到了GROUP BY，实际上是一种分组的聚合方式，已经把一组的数据聚合成为了一条记录，所以再进行排序的时候，也相当于是对分的组进行排序。

---


[leetcode](./leetcode.md)

    181. 超过经理收入的员工
    184. 部门工资最高的员工  DENSE_RANK()
    197. 上升的温度 datediff() / TIMESTAMPDIFF()
    610. 判断三角形  case when xx then x else x end
    626. 换座位  LEAD(), LAG() / IF()
    1084. 销售分析III count( sale_date between '2019-01-01' and '2019-03-31' or null ) 
    1174. 即时食物配送 II   (x, x) in (x, x) / SUM(x = x)
    1193. 每月交易 I  IF / SUM / DATE_FORMAT(date, format)
    1789. 员工的直属部门  UNION / IF()

    [180. 连续出现的数字](https://leetcode.cn/problems/consecutive-numbers)
    [550. 游戏玩法分析 IV](https://leetcode.cn/problems/game-play-analysis-iv/)
    [585. 2016年的投资](https://leetcode.cn/problems/investments-in-2016)
    [1164. 指定日期的产品价格](https://leetcode.cn/problems/product-price-at-a-given-date/)
    [1204. 最后一个能进入巴士的人](https://leetcode.cn/problems/last-person-to-fit-in-the-bus)
    
    [1280. 学生们参加各科测试的次数](https://leetcode.cn/problems/students-and-examinations/)
    [1321. 餐馆营业额变化增长](https://leetcode.cn/problems/restaurant-growth/)
    [1934. 确认率 AVG() IFNULL ](https://leetcode.cn/problems/confirmation-rate/)  

[高性能MySQL](./高性能MySQL.md)

    schema 设计与管理

        选择优化的数据类型
            整数，实数，字符串，日期和时间，位压缩，JSON，标识符，特殊数据类型
        设计中的陷阱
            太多的列，太多的联接，全能的枚举，变相的枚举，NULL不是虚拟值
        schema管理

    创建高性能索引

        索引简介
        索引策略
            前缀索引和索引的选择性，
            多列索引，
            选择合适的索引列顺序，
            聚簇索引，
            覆盖索引，
            使用索引扫描来做排序，冗余和重复索引，未使用的索引
        维护索引和表

    查询性能优化

        慢查询基础 优化数据访问
            是否向数据库请求了不需要的数据
            MySql是否在扫描额外的记录
        重构查询的方式
            一个复杂查询还是多个简单查询
            切分查询
            分解联接查询        
        查询执行的基础
            MySql的客户端/服务器通信协议
            查询状态
            查询优化处理
            查询执行引擎
            将结果返回给客户端        
        MYSQL查询优化器的局限性
            UNION的限制
            等值传递
            并行执行
            在同一个表中查询和更新
        优化特定类型的查询
            优化COUNT()
            优化联接查询
            使用 where rollup 优化 group by
            优化 limit 和 offset 子句
            优化 sql_calc_found_rows
            优化 union查询

[mysqldump](./mysqldump.md)

    mysqldump -uroot -p'xxx' -B --all-database > /home/zhang/bak/database_$(date +%F).sql
    mysqldump -uroot -p'xxx' -B gitea > /home/zhang/bak/gitea_$(date +%F).sql
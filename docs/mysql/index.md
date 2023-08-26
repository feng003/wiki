
[MySQL数据库设计规范](./MySQL数据库设计规范.md)

[MySQL数据库设计](./MySQL数据库设计.md)

[MySQL参考地址](./MySQL参考地址.md)

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

---

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

    创建高性能索引

    查询性能优化

[mysqldump](./mysqldump.md)

    mysqldump -uroot -p'xxx' -B --all-database > /home/zhang/bak/database_$(date +%F).sql
    mysqldump -uroot -p'xxx' -B gitea > /home/zhang/bak/gitea_$(date +%F).sql
## leetcode mysql

    AVG()属于聚合函数,它接受的参数可以是字段名,也可以是返回数字的表达式。
    类似的 MIN(),MAX(),SUM() 都是可以的， 特殊的count用法：
    count(age > 20 or null) 如果是 count(age > 20) = count(*) 
    对于非聚合函数如ABS(), UPPER()等,就只能接受字段作为参数.

    // 窗口函数
    SELECT score, ROW_NUMBER() OVER(order by score desc) AS 'rank' from Scores;

    SELECT S.score, DENSE_RANK() OVER (PARTITION BY id ORDER BY S.score DESC ) AS 'rank' FROM Scores S;

    // 字符串操作函数
    SUBSTRING(column_name, start, length)
    CONCAT(string1, string2, ...)

    SELECT user_id, CONCAT(UPPER(substring(name,1,1)), LOWER(substring(name,2)) ) as name 
    from Users order by user_id

    // 模糊匹配 邮箱 (^[a-zA-Z][a-zA-Z0-9_.-]*\\@leetcode\\.com$)
    SELECT * FROM Patients WHERE conditions REGEXP '\\bDIAB1.*';
    SELECT * from Patients where conditions like 'DIAB1%' or conditions like '% DIAB1%';
    SELECT * from Users where mail REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*\\@leetcode\\.com$'

    // GROUP_CONCAT() / count( distinct() )
    SELECT sell_date, count( distinct(product) ) as num_sold, 
    GROUP_CONCAT(DISTINCT product ORDER BY product SEPARATOR ',') as products 
    from Activities group by sell_date ORDER BY  sell_date ASC;

---

### 180. 连续出现的数字

    ### 按照ID升序排序下，统计num中连续重复出现3次及以上的数字

    Logs =
    | id | num |
    | -- | --- |
    | 1  | 1   |
    | 2  | 1   |
    | 3  | 1   |
    | 4  | 2   |
    | 5  | 1   |
    | 6  | 2   |
    | 7  | 2   |

    ### 答案 三个表之间的关系
    SELECT DISTINCT l1.Num AS ConsecutiveNums FROM Logs l1,Logs l2, Logs l3 
    WHERE l1.Id = l2.Id - 1 AND l2.Id = l3.Id - 1 AND l1.Num = l2.Num AND l2.Num = l3.Num ;

    ### 答案 
    ### 判断是否连续，若考虑id不连续的情况，光凭题目中的id就不够用了，我们可利用row_num函数按照id 升序对其进行连续排序，
    row_number() over(order by id) 记为rank1
    ### 统计num的重复数据，自然需要对num进行分组统计，同样对其进行排名 row_number() over(partition by Num order by Id) 记为rank2

    SELECT DISTINCT Num ConsecutiveNums FROM(SELECT *, ROW_NUMBER() OVER (PARTITION BY Num ORDER BY Id) rownum,
      ROW_NUMBER() OVER (ORDER BY Id) id2 FROM LOGS ) t
    GROUP BY (id2-rownum) ,Num HAVING COUNT(*)>=3 

### 550. 游戏玩法分析 IV

    ### 计算从首次登录日期开始至少连续两天登录的玩家的数量，然后除以玩家总数。
    Activity =
    | player_id | device_id | event_date | games_played |
    | --------- | --------- | ---------- | ------------ |
    | 1         | 2         | 2016-03-01 | 5            |
    | 1         | 2         | 2016-03-02 | 6            |
    | 2         | 3         | 2017-06-25 | 1            |
    | 3         | 1         | 2016-03-02 | 0            |
    | 3         | 4         | 2018-07-03 | 5            |

    ### 思路1
    先过滤出每个用户的首次登陆日期，然后左关联，筛选次日存在的记录的比例

    ### 答案
    SELECT round(avg(a.event_date is not null), 2) fraction from 
    (SELECT player_id, min(event_date) as login from activity group by player_id ) as p left join 
    activity as a on p.player_id=a.player_id and datediff(a.event_date, p.login)=1

    ### 答案2
    SELECT round( ((SELECT count(player_id) from 
    (SELECT player_id, datediff(event_date, min(event_date) over(partition by player_id)) as diff from activity ) as tmp 
    where diff = 1 ) / (SELECT count(distinct player_id) from activity)) ,2) as fraction;


### 585. 2016年的投资

    ### 报告 2016 年 (tiv_2016) 所有满足下述条件的投保人的投保金额之和：
        在 2015 年的投保额 (tiv_2015) 至少跟一个其他投保人在 2015 年的投保额相同。
        所在的城市必须与其他投保人都不同（也就是说 (lat, lon) 不能跟其他任何一个投保人完全相同）。

    Insurance =
    | pid | tiv_2015 | tiv_2016 | lat | lon |
    | --- | -------- | -------- | --- | --- |
    | 1   | 10       | 5        | 10  | 10  |
    | 2   | 20       | 20       | 20  | 20  |
    | 3   | 10       | 30       | 20  | 20  |
    | 4   | 10       | 40       | 40  | 40  |

    ### 思路1
    SELECT round(sum(r1.tiv_2016),2) as tiv_2016 from 
    (SELECT pid,tiv_2016 from Insurance where tiv_2015 in (SELECT tiv_2015 from Insurance group by tiv_2015 having count(1) > 1) ) as r1 join 
    ( SELECT pid,tiv_2016 from Insurance group by lat,lon having count(1) = 1 ) as r2 on r1.pid = r2.pid

    ### 思路2
    SELECT round(sum(tiv_2016),2) as tiv_2016 from Insurance 
    where tiv_2015 in ( SELECT tiv_2015 from Insurance group by tiv_2015 having count(1) > 1 ) 
    and concat(lat, lon) in (SELECT concat(lat, lon) from Insurance group by lat,lon having count(1) = 1 )

### 1164. 指定日期的产品价格 全集 JOIN 子集 缺失的值为null

    ### 查找在 2019-08-16 时全部产品的价格，假设所有产品在修改前的价格都是 10 

    Product = 
    | product_id | new_price | change_date |
    | ---------- | --------- | ----------- |
    | 1          | 20        | 2019-08-14  |
    | 2          | 50        | 2019-08-14  |
    | 1          | 30        | 2019-08-15  |
    | 1          | 35        | 2019-08-16  |
    | 2          | 65        | 2019-08-17  |
    | 3          | 20        | 2019-08-18  |

    ### 思路1
    SELECT product_id,new_price as price from Products where (product_id,change_date) in (SELECT product_id, max(change_date) from Products where change_date <= "2019-08-16" group by product_id)
    union
    (SELECT product_id, if(2>1, 10, 10) as price from Products where change_date >'2019-08-16' group by product_id having count(1) < 2 )

    ### 思路2
    SELECT product_id, price from ( SELECT product_id, new_price as price, dense_rank() over(partition by product_id order by change_date desc) as rnk from Products where change_date <= '2019-08-16' ) t where rnk = 1

    ### 答案1 全集 JOIN 子集 缺失的值为null

    SELECT p1.product_id, ifnull(p2.new_price, 10) as price from
     (SELECT distinct product_id from Products ) as p1  left join 
     (SELECT product_id,new_price from Products where (product_id,change_date) in (SELECT product_id, max(change_date) 
     from Products where change_date <= "2019-08-16" group by product_id) ) as p2
     on p1.product_id=p2.product_id

     ### 答案2 COALESCE(expr1, expr2, ..., default_value) 
     ### COALESCE函数是用于从一组表达式中返回第一个非空的值的非常有用的函数，特别适用于处理多个列或表达式的情况
     SELECT distinct p1.product_id, coalesce(( SELECT p2.new_price from Products as p2 where p2.change_date <= '2019-08-16' and p1.product_id=p2.product_id order by p2.change_date desc limit 1 )  ,10) as price 
     from Products as p1

---

### 181. 超过经理收入的员工

    table Employee

    | id | name  | salary | managerId |
    | -- | ----- | ------ | --------- |
    | 1  | Joe   | 70000  | 3         |
    | 2  | Henry | 80000  | 4         |
    | 3  | Sam   | 60000  | null      |
    | 4  | MAX   | 90000  | null      |

    ### join
    SELECT e2.*, e1.* from Employee as e1 LEFT JOIN Employee as e2 
    on e2.managerId = e1.id WHERE e2.managerId is not null 

    | id | name  | salary | managerId | id | name | salary | managerId |
    | -- | ----- | ------ | --------- | -- | ---- | ------ | --------- |
    | 1  | Joe   | 70000  | 3         | 3  | Sam  | 60000  | null      |
    | 2  | Henry | 80000  | 4         | 4  | MAX  | 90000  | null      |

    SELECT e2.name as Employee from Employee as e1 
    LEFT JOIN Employee as e2 on e2.managerId = e1.id 
    WHERE e2.managerId is not null and e2.salary > e1.salary;

    ###答案 WHERE 
    SELECT a.Name AS 'Employee' FROM Employee AS a, Employee AS b 
    WHERE a.ManagerId = b.Id AND a.Salary > b.Salary;

### 184. 部门工资最高的员工  DENSE_RANK()

    Employee = 
    | id | name  | salary | departmentId |
    | -- | ----- | ------ | ------------ |
    | 1  | Joe   | 70000  | 1            |
    | 2  | Jim   | 90000  | 1            |
    | 3  | Henry | 80000  | 2            |
    | 4  | Sam   | 60000  | 2            |
    | 5  | MAX   | 90000  | 1            |

    Department = 
    | id | name  |
    | -- | ----- |
    | 1  | IT    |
    | 2  | Sales |

    | Department | Employee | Salary |
    | ---------- | -------- | ------ |
    | IT         | Jim      | 90000  |
    | Sales      | Henry    | 80000  |
    | IT         | MAX      | 90000  |

    ###答案 DENSE_RANK()
    SELECT d.name as Department, e.name as Employee, e.salary as Salary from Department as d 
    LEFT JOIN ( SELECT *, DENSE_RANK() OVER(PARTITION by departmentId order by salary desc) as 'rank' from Employee ) as e 
    on d.id = e.departmentId and e.rank=1 WHERE e.salary is not null

    ###答案 join  and (x, x) in (x, x)
    SELECT Department.name AS 'Department', Employee.name AS 'Employee',Salary FROM Employee 
    JOIN  Department ON Employee.DepartmentId = Department.Id 
    WHERE (Employee.DepartmentId , Salary) IN (SELECT DepartmentId, MAX(Salary) FROM Employee GROUP BY DepartmentId )


### 197. 上升的温度 datediff() / TIMESTAMPDIFF()

    Weather = 
    | id | recordDate | temperature |
    | -- | ---------- | ----------- |
    | 1  | 2015-01-01 | 10          |
    | 2  | 2015-01-02 | 25          |
    | 3  | 2015-01-03 | 20          |
    | 4  | 2015-01-04 | 30          |

    ###答案 datediff()  与之前的日期相比 温度更高的所有日期
    SELECT w1.id as Id from Weather w1 , Weather w2  
    WHERE datediff(w1.recordDate, w2.recordDate) = 1 and  w1.temperature > w2.temperature ;

    ###答案 TIMESTAMPDIFF()
    SELECT w1.Id from Weather as w1, Weather as w2 
    WHERE TIMESTAMPDIFF(DAY, w2.RecordDate, w1.RecordDate) = 1 AND w1.Temperature > w2.Temperature

### 610. 判断三角形  case when xx then x else x end

    | x  | y  | z  |
    | -- | -- | -- |
    | 13 | 15 | 30 |
    | 10 | 20 | 15 |

    ### 答案
    SELECT x,y,z, (case when x+y >z and x+z >y and y+z >x THEN 'Yes' ELSE 'No' END) as 'triangle' from triangle;

### 626. 换座位  LEAD(), LAG() / IF()

    Seat =
    | id | student |
    | -- | ------- |
    | 1  | Abbot   |
    | 2  | Doris   |
    | 3  | Emerson |
    | 4  | Green   |
    | 5  | Jeames  |
    ### 交换每两个连续的学生的座位号。如果学生的数量是奇数，则最后一个学生的id不交换。

    ### LEAD(column_name, offset, default_value) OVER (PARTITION BY partition_expression ORDER BY sort_expression)  获取当前行后面的指定行数的值
    ### LAG(column_name, offset, default_value) OVER (PARTITION BY partition_expression ORDER BY sort_expression)  获取当前行前面的指定行数的值

    SELECT id, IF(id & 1, LEAD(student,1, student) OVER(), LAG(student, 1) OVER() ) AS student
    FROM Seat

    ### 答案  IF()
    SELECT if( id % 2 = 0, id-1, if( id = (SELECT max(id) from Seat ), id, id+1) ) as id, student from Seat order by id;

    ### 答案
    SELECT a.id as id,ifnull(b.student,a.student) as student from Seat as a left join 
    ( SELECT * from Seat where mod(id,2) = 0 ) as b on (a.id+1) = b.id where mod(a.id,2) = 1
    union
    SELECT c.id as id,d.student as student from Seat as c left join 
    ( SELECT * from Seat where mod(id,2) = 1 ) as d on (c.id-1) = d.id where mod(c.id,2) = 0
    order by id asc;


### 1084. 销售分析III count( sale_date between '2019-01-01' and '2019-03-31' or null ) 

    | product_id | product_name | unit_price |
    | ---------- | ------------ | ---------- |
    | 1          | S8           | 1000       |
    | 2          | G4           | 800        |
    | 3          | iPhone       | 1400       |

    ## 所有售出日期都在这个时间内 = 在这个时间内售出的商品数量等于总商品数量

    ###答案  count( sale_date between '2019-01-01' and '2019-03-31' or null ) 
    SELECT p.product_id, p.product_name from Sales as s left join Product as p on p.product_id=s.product_id 
    group by product_id
    having count( sale_date between '2019-01-01' and '2019-03-31' or null ) = count(*)
    <!-- having SUM(s.sale_date BETWEEN '2019-01-01' AND '2019-03-31') = COUNT(*) -->

    ### 补集的思想：NOT IN
    SELECT product_id, product_name from product  where product_id NOT IN 
    (SELECT s.product_id from sales s where sale_date < '2019-01-01' or sale_date > '2019-03-31' )
    and product_id in ( SELECT s.product_id from sales s )

### 1174. 即时食物配送 II   (x, x) in (x, x) / SUM(x = x)

    ## 获取即时订单在所有用户的首次订单中的比例
    ### 即时订单 (order_date = customer_pref_delivery_date)，否则称为 计划订单
    ### 首次订单是顾客最早创建的订单,只有一个

    Delivery =
    | delivery_id | customer_id | order_date | customer_pref_delivery_date |
    | ----------- | ----------- | ---------- | --------------------------- |
    | 1           | 1           | 2019-08-01 | 2019-08-02                  |
    | 2           | 2           | 2019-08-02 | 2019-08-02                  |
    | 3           | 1           | 2019-08-11 | 2019-08-12                  |
    | 4           | 3           | 2019-08-24 | 2019-08-24                  |
    | 5           | 3           | 2019-08-21 | 2019-08-22                  |
    | 6           | 2           | 2019-08-11 | 2019-08-13                  |
    | 7           | 4           | 2019-08-09 | 2019-08-09                  |

    ###答案 (x, x) in (x, x)
    SELECT round( SUM(order_date = customer_pref_delivery_date) /COUNT(1) *100 , 2) as immediate_percentage 
    from Delivery where ( customer_id, order_date) in 
    ( SELECT customer_id,MIN(order_date) from Delivery group by customer_id )

### 1193. 每月交易 I  IF / SUM / DATE_FORMAT(date, format)

    ### 查询来查找每个月和每个国家/地区的事务数及其总金额、已批准的事务数及其总金额。
    Transactions =
    | id  | country | state    | amount | trans_date |
    | --- | ------- | -------- | ------ | ---------- |
    | 121 | US      | approved | 1000   | 2018-12-18 |
    | 122 | US      | declined | 2000   | 2018-12-19 |
    | 123 | US      | approved | 2000   | 2019-01-01 |
    | 124 | DE      | approved | 2000   | 2019-01-07 |

    ### 错误思路  if(state='approved', +amount, 0) 
    SELECT DATE_FORMAT(trans_date, '%Y-%m') AS month, country ,COUNT(1) as trans_COUNT, SUM(state='approved') as approved_COUNT, SUM(amount) as trans_total_amount, if(state='approved', +amount, 0)  as approved_total_amount from Transactions group by month,country;

    ### 答案   SUM(if(state='approved', amount, 0)) / SUM(state='approved')
    SELECT DATE_FORMAT(trans_date, '%Y-%m') AS month, country ,COUNT(1) as trans_COUNT, SUM(state='approved') as approved_COUNT, SUM(amount) as trans_total_amount, SUM(if(state='approved', amount, 0))  as approved_total_amount from Transactions group by month,country;

### 1321. 餐馆营业额变化增长

    ### 计算以 7 天（某日期 + 该日期前的 6 天）为一个时间段的顾客消费平均值
    Customer =
    | customer_id | name    | visited_on | amount |
    | ----------- | ------- | ---------- | ------ |
    | 1           | Jhon    | 2019-01-01 | 100    |
    | 2           | Daniel  | 2019-01-02 | 110    |
    | 3           | Jade    | 2019-01-03 | 120    |
    | 4           | Khaled  | 2019-01-04 | 130    |
    | 5           | Winston | 2019-01-05 | 110    |
    | 6           | Elvis   | 2019-01-06 | 140    |
    | 7           | Anna    | 2019-01-07 | 150    |
    | 8           | Maria   | 2019-01-08 | 80     |
    | 9           | Jaze    | 2019-01-09 | 110    |
    | 1           | Jhon    | 2019-01-10 | 130    |
    | 3           | Jade    | 2019-01-10 | 150    |
    

### 1789. 员工的直属部门  UNION / IF()

    ### 查出员工所属的直属部门

    Employee =
    | employee_id | department_id | primary_flag |
    | ----------- | ------------- | ------------ |
    | 1           | 1             | N            |
    | 2           | 1             | Y            |
    | 2           | 2             | N            |
    | 3           | 3             | N            |
    | 4           | 2             | N            |
    | 4           | 3             | Y            |
    | 4           | 4             | N            |

    ### 答案 union
    SELECT employee_id, department_id from Employee  group by employee_id having count(department_id) = 1
    union 
    SELECT employee_id, department_id from Employee where primary_flag = 'Y';

    ### 答案 if(condition, Y, N)
    SELECT e1.employee_id,if(count(e1.department_id) < 2,e1.department_id,(SELECT department_id from Employee e2 
    where e2.employee_id = e1.employee_id and e2.primary_flag = 'Y')) department_id
    from Employee e1 group by e1.employee_id having department_id is not null;

### 1934. 确认率  AVG() IFNULL

    Signups =
    | user_id | time_stamp          |
    | ------- | ------------------- |
    | 3       | 2020-03-21 10:16:13 |
    | 7       | 2020-01-04 13:57:59 |
    | 2       | 2020-07-29 23:09:44 |
    | 6       | 2020-12-09 10:39:37 |

    Confirmations =
    | user_id | time_stamp          | action    |
    | ------- | ------------------- | --------- |
    | 3       | 2021-01-06 03:30:46 | timeout   |
    | 3       | 2021-07-14 14:00:00 | timeout   |
    | 7       | 2021-06-12 11:57:29 | confirmed |
    | 7       | 2021-06-13 12:58:28 | confirmed |
    | 7       | 2021-06-14 13:59:27 | confirmed |
    | 2       | 2021-01-22 00:00:00 | confirmed |
    | 2       | 2021-02-28 23:59:59 | timeout   |

    ### 思路
    SELECT * from Signups as s LEFT JOIN Confirmations as c on s.user_id=c.user_id;
    
    SELECT user_id, COUNT(1) as COUNT,action from Confirmations group by user_id,action

    ### 答案  AVG(c.action='confirmed')
    SELECT s.user_id, ROUND(IFNULL(AVG(c.action='confirmed'), 0), 2) AS confirmation_rate
    FROM Signups AS s LEFT JOIN Confirmations AS c ON s.user_id = c.user_id GROUP BY s.user_id
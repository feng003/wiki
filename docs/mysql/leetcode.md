## leetcode mysql

    AVG()属于聚合函数,它接受的参数可以是字段名,也可以是返回数字的表达式。
    类似的 MIN(),MAX(),SUM() 都是可以的， 特殊的count用法：
    count(age > 20 or null) 如果是 count(age > 20) = count(*) 
    对于非聚合函数如ABS(), UPPER()等,就只能接受字段作为参数:

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

### 1084. 销售分析III count( sale_date between '2019-01-01' and '2019-03-31' or null ) 

    | product_id | product_name | unit_price |
    | ---------- | ------------ | ---------- |
    | 1          | S8           | 1000       |
    | 2          | G4           | 800        |
    | 3          | iPhone       | 1400       |

    ## 所有售出日期都在这个时间内 = 在这个时间内售出的商品数量等于总商品数量

    ###答案  count( sale_date between '2019-01-01' and '2019-03-31' or null ) 
    select p.product_id, p.product_name from Sales as s left join Product as p on p.product_id=s.product_id 
    group by product_id
    having count( sale_date between '2019-01-01' and '2019-03-31' or null ) = count(*)
    <!-- having SUM(s.sale_date BETWEEN '2019-01-01' AND '2019-03-31') = COUNT(*) -->

    ### 补集的思想：NOT IN
    select product_id, product_name from product  where product_id NOT IN 
    (select s.product_id from sales s where sale_date < '2019-01-01' or sale_date > '2019-03-31' )
    and product_id in ( select s.product_id from sales s )

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
    select round( SUM(order_date = customer_pref_delivery_date) /COUNT(1) *100 , 2) as immediate_percentage 
    from Delivery where ( customer_id, order_date) in 
    ( select customer_id,MIN(order_date) from Delivery group by customer_id )

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
    select DATE_FORMAT(trans_date, '%Y-%m') AS month, country ,COUNT(1) as trans_COUNT, SUM(state='approved') as approved_COUNT, SUM(amount) as trans_total_amount, if(state='approved', +amount, 0)  as approved_total_amount from Transactions group by month,country;

    ### 答案   SUM(if(state='approved', amount, 0)) / SUM(state='approved')
    select DATE_FORMAT(trans_date, '%Y-%m') AS month, country ,COUNT(1) as trans_COUNT, SUM(state='approved') as approved_COUNT, SUM(amount) as trans_total_amount, SUM(if(state='approved', amount, 0))  as approved_total_amount from Transactions group by month,country;
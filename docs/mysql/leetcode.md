## leetcode mysql

### 181. 超过经理收入的员工  

    table Employee

    | id | name  | salary | managerId |
    | -- | ----- | ------ | --------- |
    | 1  | Joe   | 70000  | 3         |
    | 2  | Henry | 80000  | 4         |
    | 3  | Sam   | 60000  | null      |
    | 4  | Max   | 90000  | null      |

    ### join
    SELECT e2.*, e1.* from Employee as e1 left join Employee as e2 
    on e2.managerId = e1.id WHERE e2.managerId is not null 

    | id | name  | salary | managerId | id | name | salary | managerId |
    | -- | ----- | ------ | --------- | -- | ---- | ------ | --------- |
    | 1  | Joe   | 70000  | 3         | 3  | Sam  | 60000  | null      |
    | 2  | Henry | 80000  | 4         | 4  | Max  | 90000  | null      |

    SELECT e2.name as Employee from Employee as e1 
    left join Employee as e2 on e2.managerId = e1.id 
    WHERE e2.managerId is not null and e2.salary > e1.salary;

    ### WHERE 
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
    | 5  | Max   | 90000  | 1            |

    Department = 
    | id | name  |
    | -- | ----- |
    | 1  | IT    |
    | 2  | Sales |

    | Department | Employee | Salary |
    | ---------- | -------- | ------ |
    | IT         | Jim      | 90000  |
    | Sales      | Henry    | 80000  |
    | IT         | Max      | 90000  |

    ### DENSE_RANK
    SELECT d.name as Department, e.name as Employee, e.salary as Salary from Department as d 
    left join ( SELECT *, DENSE_RANK() OVER(PARTITION by departmentId order by salary desc) as 'rank' from Employee ) as e 
    on d.id = e.departmentId and e.rank=1 WHERE e.salary is not null

    ### join  and in (x, x)
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

    ### datediff()  与之前的日期相比 温度更高的所有日期
    SELECT w1.id as Id from Weather w1 , Weather w2  
    WHERE datediff(w1.recordDate, w2.recordDate) =1 and  w1.temperature > w2.temperature ;

    ### TIMESTAMPDIFF()
    SELECT w1.Id from Weather as w1, Weather as w2 
    WHERE TIMESTAMPDIFF(DAY, w2.RecordDate, w1.RecordDate) = 1 AND w1.Temperature > w2.Temperature

### 1934. 确认率  AVG IFNULL

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
    SELECT * from Signups as s left join Confirmations as c on s.user_id=c.user_id;
    
    SELECT user_id, count(1) as count,action from Confirmations group by user_id,action

    ### 答案  AVG() 用法
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
    select DATE_FORMAT(trans_date, '%Y-%m') AS month, country ,count(1) as trans_count, SUM(state='approved') as approved_count, SUM(amount) as trans_total_amount, if(state='approved', +amount, 0)  as approved_total_amount from Transactions group by month,country;


    ### 答案   SUM(if(state='approved', amount, 0)) / SUM(state='approved')
    select DATE_FORMAT(trans_date, '%Y-%m') AS month, country ,count(1) as trans_count, SUM(state='approved') as approved_count, SUM(amount) as trans_total_amount, SUM(if(state='approved', amount, 0))  as approved_total_amount from Transactions group by month,country;
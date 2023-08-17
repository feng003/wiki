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
    select e2.*, e1.* from Employee as e1 left join Employee as e2 on e2.managerId = e1.id where e2.managerId is not null 

    | id | name  | salary | managerId | id | name | salary | managerId |
    | -- | ----- | ------ | --------- | -- | ---- | ------ | --------- |
    | 1  | Joe   | 70000  | 3         | 3  | Sam  | 60000  | null      |
    | 2  | Henry | 80000  | 4         | 4  | Max  | 90000  | null      |

    select e2.name as Employee from Employee as e1 left join Employee as e2 on e2.managerId = e1.id where e2.managerId is not null and e2.salary > e1.salary;

    ### where 
    SELECT a.Name AS 'Employee' FROM Employee AS a, Employee AS b WHERE a.ManagerId = b.Id AND a.Salary > b.Salary;

### 184. 部门工资最高的员工

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
    select d.name as Department, e.name as Employee, e.salary as Salary from Department as d left join ( select *, DENSE_RANK() OVER(PARTITION by departmentId order by salary desc) as 'rank' from Employee ) as e on d.id = e.departmentId and e.rank=1 where e.salary is not null

    ### join  and in (x, x)
    SELECT Department.name AS 'Department', Employee.name AS 'Employee',Salary FROM Employee JOIN  Department ON Employee.DepartmentId = Department.Id WHERE (Employee.DepartmentId , Salary) IN (SELECT DepartmentId, MAX(Salary) FROM Employee GROUP BY DepartmentId )


### 197. 上升的温度

    Weather = 
    | id | recordDate | temperature |
    | -- | ---------- | ----------- |
    | 1  | 2015-01-01 | 10          |
    | 2  | 2015-01-02 | 25          |
    | 3  | 2015-01-03 | 20          |
    | 4  | 2015-01-04 | 30          |

    ### datediff()  与之前的日期相比 温度更高的所有日期
    select w1.id as Id from Weather w1 , Weather w2  where datediff(w1.recordDate, w2.recordDate) =1 and  w1.temperature > w2.temperature ;

    ### TIMESTAMPDIFF()
    select w1.Id from Weather as w1, Weather as w2 where TIMESTAMPDIFF(DAY, w2.RecordDate, w1.RecordDate) = 1 AND w1.Temperature > w2.Temperature

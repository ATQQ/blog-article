# 1. 查询

## 检索所有行和列
```sql
select * from table

select * from employee
```

## 查询部分列
```sql
select col1,col2,col3 from table

select fname,mint from employee
```

## 使用友好列标题查询(更换查询出来后的表头名称)
```sql
select col1 as 列1,col2 as 列2 from table

select fname as 名字,mint as 代号 from employee
```

## 检索前n行数据
```sql
-- SQL Server
select top n * from table

select top 3 * from employee
-- MySQL
select * from table limit 0,n

select * from employee limit 0,3
```

## 检索前%n行数据
```sql
-- SQL Server
select top n percent * from table

select top 3 percent * from employee
```

## 指定检索条件
```sql
select * from table where col1=content

SELECT * FROM employee WHERE sex='M'
-- 多条件
select * FROM employee WHERE sex= 'M' and salary> 10000

select * FROM employee WHERE sex="F" or salary <20000
```

## 区间查询
```sql
select * from employee where salary BETWEEN 20000 and 30000

select * from employee where salary not BETWEEN 20000 and 30000

-- 特定的取值区间
SELECT * FROM employee WHERE fname in ('james','sam','Frandin')
```

## 筛选1列或多列不重复的数据
```sql
select distinct col1,col2,... table

SELECT DISTINCT sex FROM employee

select distinct age from employee
```

## 模糊查询
```sql
SELECT * FROM employee WHERE fname LIKE 'J%'

SELECT * FROM employee WHERE fname LIKE '%a%'

SELECT * FROM employee WHERE fname LIKE '__i%'
-- 正则表达式查询匹配
select * from tablename where colname regexp '正则表达式'
```

## 查询null数据
```sql
SELECT * FROM employee WHERE salary IS NULL

SELECT * FROM employee WHERE salary is NOT NULL
```

## 对查询的数据进行排序
```sql
SELECT * from employee ORDER BY salary ASC

SELECT * from employee ORDER BY salary desc

SELECT * from employee ORDER BY salary DESC,bdate ASC
```
## 算数表达式
```sql
SELECT fname,salary,salary/30 AS daySalary FROM employee
```
## 函数
```sql
-- count函数(统计插叙数据的数量)
select count(*)as 数量 FROM employee WHERE salary>40000

-- max,min,avg函数
SELECT max(salary) as max,min(salary)as min,avg(salary) as average FROM employee

-- sum求和函数
select sum(hours) FROM works_on WHERE pno=1
```


## 数据分组(group by)
group by 对原始数据进行分组 having 对分组后的数据进行筛选(先有group by 后having)
```sql
SELECT sex,count(sex) FROM employee  GROUP BY sex HAVING count(sex) >4

SELECT salary,COUNT(ssn) FROM employee GROUP BY salary 
```
## 多表查询
### 链接查询分类
* 内连接
```sql
-- 查询每个员工的工作时长
SELECT employee.fname,employee.address,sum(works_on.hours) FROM works_on JOIN employee  ON works_on.essn=employee.ssn GROUP BY fname;

SELECT dname,dnumber,COUNT(dno)  FROM  department JOIN employee on dno=dnumber GROUP BY dname;

SELECT fname,dname FROM  employee,department WHERE dno=dnumber
```
* 外连接 
    * 左外连接
    ```sql
    SELECT fname,address,dependent_name FROM employee LEFT JOIN dependent ON ssn=essn;
    ```    
    * 右外连接
    ```sql
    SELECT fname,address,dependent_name FROM employee right JOIN dependent ON ssn=essn;
    ```
    * 全外连接
    ```sql
    SELECT fname,address,dependent_name FROM employee full JOIN dependent ON ssn=essn;
    ```
* 交叉连接
```sql
SELECT * from employee CROSS JOIN department
```

## 嵌套查询
### In
```sql
SELECT * FROM employee WHERE ssn in  (SELECT essn FROM dependent);

SELECT * FROM employee WHERE ssn not in  (SELECT DISTINCT essn FROM dependent);
```
### all
```sql
SELECT * FROM employee WHERE sex='M' and salary> all(SELECT  salary FROM employee where sex='F')
-- 等价于
SELECT * FROM employee WHERE sex='M' and salary> (SELECT max( salary )FROM employee where sex='F')
```

### exists
```sql
SELECT * FROM employee WHERE not EXISTS (SELECT * FROM dependent WHERE ssn=essn )
```

## 集合查询
### 并运算
前提条件：select的列数目相等且类型兼容
* union 剔除重复记录
* unionall 保留重复记录
```sql
SELECT  fname FROM employee UNION SELECT dependent_name from dependent;
```

### 交运算
```sql
-- sql server
select fname from user where ses='M' intersect
select fname from user where age>20; 
```

## 差运算
```sql
select fname from user where ses='M' except
select fname from user where age>20; 
```
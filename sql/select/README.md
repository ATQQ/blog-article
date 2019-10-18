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
select * from table where col1="content"

SELECT * FROM employee WHERE sex="M"
```
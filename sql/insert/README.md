# 2. 插入

```sql
-- 对所有字段赋值，省略字段名称
INSERT INTO `employee` VALUES ('John', 'B', 'Smith ', '123456789', '1965-01-09 00:00:00', '731 Fondren, Houston. TX', 'M', 30000, '333445555', '5');

-- 指定字段
INSERT INTO `employee`(fname,ssn) VALUES ('demo', '123456987');

-- 插入子查询结果
INSERT INTO `employee` (fname,sex) (SELECT dependent_name,sex from dependent);

-- 查询创建表
    -- mysql
    create table new_table_name as select * from (your_sql) as sql_table;

    create table test as select * from works_on as test;

    -- sql Server
    select * into test from works_on
```
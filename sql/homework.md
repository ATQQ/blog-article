# 5. 期末复习

1. 要保证数据库的逻辑数据独立性，可能需要修改的是$\color{yellow}{模式与外模式之间的映象}$
2. 三级模式$\color{yellow}{外模式、概念模式、内模式}$
3. 视图属于$\color{yellow}{外模式}$
4. =ANT等价于IN
5. ER模型三要素 $\color{yellow}{实体,属性,联系}$
6. 视图是$\color{yellow}{虚表}$
7. 关系模式进行规范化是数据库设计中$\color{yellow}{逻辑设计}$阶段任务
8. 自然连接是等值链接,两个自然连接属性必然有同名属性
9. 关于关系代数中的“并相容性”,两个关系的并集要兼容
10. π对应于Select
11. 3NF是在2NF的基础上，消除了非主属性对码的传递函数依赖
12. 关系操作的特点是集合操作
13. 外模式又称用户模式或子模式
14. 数据库管理系统在三个模式之间提供了两层映像，即（外模式/模式映像）、（模式/内模式映像）。
15. 事务的特征:原子性,一致性,持久性,隔离性

## 简答题
### 1.数据独立性指的是什么？它能带来哪些好处？ 
  
答：数据独立性包括逻辑独立性和物理独立性两部分。物理独立性是指当数据的存储结构发生变化时，不影响应用程序的特性；逻辑独立性是指当表达现实世界的信息内容发生变化时，不影响应用程序的特性。这两个独立性使用户只需关心逻辑层即可，同时增强了应用程序的可维护性

### 2.简述数据库系统的组成以及各部分的作用。其核心部分是什么？
由数据库、数据库管理系统（及其开发工具）、应用系统、数据库管理员（和用户）构成。

数据库管理系统

### 3,SQL SERVER 2008支持的数据完整性有哪几类?

域完整性，实体完整性，参照完整性，用户定义完整性

### 4.是否可以更新视图
由一个基表定义的视图，只含有基表的主键或候补键，并且视图中没有用表达式或函数定义的属性，才允许更新

* 若视图的字段是来自字段表达式或常数，则不允许对此视图执行INSERT、UPDATE操作，允许执行DELETE操作；
* 字段是来自库函数不允许更新
* 定义中有GROUP BY子句或聚集函数时不允许更新
* 定义中有DISTINCT任选项，则此视图不允许更新；
* 定义中有嵌套查询,并且嵌套查询的FROM子句中涉及的表也是导出该视图的基表，则此视图不允许更新；
* 若视图是由两个以上的基表导出的，此视图不允许更新；
* 一个不允许更新的视图上定义的视图也不允许更新；

## 复杂关键字
```sql
-- 排序
order by ASC/DESC
-- 去重
distinct 
-- 主键
primary key
-- 外建
foreign key(col_name) references other_table(col_name)

-- 约束
constraint 
-- 授权
grant
-- 撤权
revoke
-- 拒绝
deny
-- 存储过程
procedure
--调用
execute
```

## 语句
```sql
-- 增加主键
ALTER TABLE works_on ADD CONSTRAINT pk_works_on PRIMARY KEY(essn,pno)

-- 增加外检约束
ALTER TABLE works_on ADD constraint foregin_key_pno FOREIGN key(pno) REFERENCES project(pnumber)
-- 删除约束
alter table tableName drop constraint c_name

-- 给予数据库权限
use STUDENT
go
CREATE USER Mike

-- 授予视图更新权限
GRANT SELECT,INSERT,DELETE,UPDATE 
   ON view_name TO Mike

-- 授予创建表权限
GRANT CREATE TABLE TO SOMEONE

-- 授予更新表的权限
GRANT SELECT,DELETE,UPDATE,INSERT ON SOME_TABLE TO SOME_ONE

-- 新建sql server登录账号
CREATE LOGIN USERNAME WITH PASSWORD='A123456'
-- 新建win账号
create login [username] from windows
-- 使之成为合法用户
CREATE USER USERNAME
create user username2 for login username1
-- 删除数据库用户
drop user username

--  对象权限
-- 授予权限
grant
grant select,delete on table_name to someBody
-- 收回权限
revoke
revoke select,delete on table_name from someBody
-- 拒绝权限
deny
deny select,delete on table_name to someBody

-- 回收权限
REVOKE SELECT,DELETE ON SOME_TABLE FROM SOME_ONE

--  语句权限
grant create table to someBody

grant create table,create view to user1,user2

revoke create table from someBody

deny create table to someBody

-- 角色相关
create role role_name

-- 为角色添加成员
exec sp_addrolemember 'role_name','user_name'
-- 为角色删除成员
exec sp_droprolemember 'role_name','user_name'
```


## T-SQL编程
1. 定义一个tinyint的整型变量，为其赋值45，并显示变量的值
```sql
DECLARE @temp tinyint
SET @temp=45
PRINT @temp
```

2. 定义一个长度为20的可变长度型字符变量，为其赋值“Welcome to SWPU”， 并显示变量的值。
```sql
DECLARE @str varchar(20)
SET @str='Welcome to SWPU'
PRINT @str
```
3. 查询服务器名
```sql
select @@servername
```
4. 查询当前数据库管理系统版本
```sql
select @@version
```
## 函数

1. 绝对值
```sql
select abs(-3) as '绝对值'
```
2. 平方根
```sql
select sqrt(4) as '平方根'
```
3. 三次方
```sql
select power(5,3) as '5的3次方'
```

## 简单的存储过程
```sql
-- 创建存储过程
create procedure demo1
@param1 int
as
select c1,c2 from table_name where c_id=@param1
-- 调用
execute demo1 @p1=23

```


```sql
procedure
procedure
```
# 3. 修改

```sql
-- 普通修改
UPDATE  test SET hours=12.1 WHERE  essn ='123456789' 

-- 带子查询的修改
UPDATE test SET hours = (SELECT MAX(hours) FROM works_on) WHERE essn = '123456789'
```
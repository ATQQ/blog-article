# 4. 删除

```sql
-- 普通删除
DELETE FROM test WHERE hours =40 LIMIT 1 

-- 带子查询删除
DELETE FROM test WHERE hours in (41)

DELETE FROM test WHERE hours not in (select DISTINCT hours from works_on)
```
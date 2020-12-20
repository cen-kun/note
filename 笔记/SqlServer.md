# SqlServer

### 分组查询

基本语法

select ....

where .....
group by 列名,列名   结合聚合函数，根据一列或多个列对结果集进行分组。

在select指定的字段要么就要包含在Group By语句的后面，作为分组的依据；要么就要被包含在聚合函数中

![1603810395115](%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/1603810395115.png)

```sql
--group by 分组
-- 语法:select ... where ...  group by ....  order by ....
--统计各部分有多少个用户
--select 出现的列名,必须出现在group by之后或包含在聚合函数中
Select DeptId,count(1) 用户数 from UserInfos
--where Age>26
group by DeptId
having DeptId>1  --分组后的筛选条件
order by DeptId desc
```


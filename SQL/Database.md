# Database



## Basic

comment: `--comment.....` `/*.........*/`

create a database: `CREATE DATABASE database_name;`

create a table: ``

```
CREATE TABLE table_name (column1_name data_type constraints....);
```

data type:

| Type      | Description                       |
| --------- | --------------------------------- |
| INT       | -2^32 ~ 2^32-1                    |
| DECIMAL   |                                   |
| CHAR      | fixed length, max 225 byte string |
| VARCHAR   | elastic length string             |
| TEXT      | max 65535 byte string             |
| DATE      | YYYY-MM-DD                        |
| DATETIME  | YYYY-MM-DD HH:MM:SS               |
| TIMESTAMP | store the timestamp               |

### SELECT

select distinct: remove the duplicate rows

line alias: don't use in where! but ok in having, group by,order by

```mysql
SELECT [column_1 | expression] AS descriptive_name / `descriptive name`
FROM table_name; 
```

table alias: `table_name (AS) table_alias `

### ORDER BY

ASC increase; DESC decrease

```mysql
SELECT column1, column2,...
FROM tbl
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC],... 
```

customize order: 

```mysql
ORDER BY FIELD(column_name, val1, val2, val3, ...); 
```

### FIELD()

return the index of the value

`FIELD(value, val1, val2, val3, ...)`

### WHERE

`WHERE search_condition; `

IN: `WHERE (expr|column_1) IN ('value1','value2',...); `

BETWEEN: `expr [NOT] BETWEEN begin_expr AND end_expr; `

LIKE: `expression LIKE pattern ESCAPE escape_character `

- 百分号（`%`）通配符匹配任何零个或多个字符的字符串。
- 下划线（`_`）通配符匹配任何单个字符。

ISNULL:`value IS NULL `

IFNULL: `IFNULL(expression, alt_value)`

### LIMIT

`LIMIT offset , count; `the first n items or nth item

### JOIN

CROSS JOIN: all combinations

(INNER) JOIN: intersection part

```mysql
SELECT column_list
FROM t1
INNER JOIN t2 ON join_condition; / USING (column)---if same name
```

LEFT JOIN: left table complete

RIGHT JOIN: right table complete

### GROUP BY

after from and where

HAVING: filter for inner group, using with group by

roll up: generate the sum up for the group by items.  

group by a, b, c rollup === group by a, b,c 

group by 1: the first select item

### CONCAT

CONCAT (str1, str2,...)

group_concat( distinct 要连接的字段 order by 排序字段 asc/desc   separator '分隔符' )

### UNION

row union, don't use or, use union instead to enhance the efficiency

### DELETE, DROP, TRUNCATE

 `DELETE FROM table_name WHERE condition;`

### EXIST

```mysql
SELECT 
    select_list
FROM
    a_table
WHERE
    [NOT] EXISTS(subquery); 
```

using exists to check the subquery. If null, return false.

the efficiency of in and exist: in MySQL 8.0, no significant difference

UPDATE

alter



### window function

rank: rank(), dense_rank(), row_number()

```mysql
window_function_name(expression) 
    OVER (
        [partition_defintion]
        [order_definition]
        [frame_definition]
    ) 
```

### case

```mysql
CASE  case_expression
   WHEN when_expression_1 THEN commands
   WHEN when_expression_2 THEN commands
   ...
   ELSE commands
END CASE; 
```

```mysql
CASE
    WHEN condition_1 THEN commands
    WHEN condition_2 THEN commands
    ...
    ELSE commands
END CASE; 
```

# rank dense_rank 用法

- rank 不能直接命名成rank，必须加双引号as "rank"
- 不能直接用where，要在外面套用一层select 再用where
- 区别rank 和dense_rank, dense 连续编号
# Distinct 用法
count(distinct xxx)
实际上可以用在任何地方
有很多问题distinct 是关键
**distinct 两列： distinct a, b**

# Left join, inner join, cross join
using(id)  
on [a.id](http://a.id) = b.id
join 有可能不止依赖一个条件，用and 隔开
很难想到用join的一道题，一列表示若干个点的坐标，求最短的线段。明显一个线段由两个点组成，所以把一个表当成两个表join
反复join 的另一个例题，1440，
```sql
select a.xxx , (v1.value > v2.value then xxx)
from tableA a left join tableB v1 on a.left = [v1.name](http://v1.name) left join tableB v2 on a.right = v2.name
```
# Function
begin()里面不加分号
# dual 用法
虚拟表
用于增加返回空
select (select xxx from) as "column" from dual, 这里from dual 可以不写
# 聚类函数用法
当聚类函数用在where中时，
例子：
错：where salary < max(salary)
对：where salary < (select max(salary) from xxx)
这里需要区分where 和 having的用法
带聚类函数的要用在having中
# null相关
- isnull ifnull nullif 区别：isnull(xxx) 返回0或1； ifnull(e1, e2)   如果e1是null返回e2； nullif(e1,e2)比较e1和e2是否相等，如果等返回null，不等返回e1
- where xx is not null
# 技巧：把一张表当两张表用
from Table a, Table b where [a.id](http://a.id) = b.subid and xxxx
# in 语法
where （a, b） in (select a, b from xxx)
# delete 语句
语法：delete from Table where xxxxx
注意：delete 在mysql中不可以直接使用where接过滤条件，需要添加中间表。
```sql
delete from Person where
id not in
(select t.* from (select min(id) from Person group by email) t);
```
这里t就是中间表
# time 相关
如果只有date，可以直接写 between "2013-10-01" and "2013-10-03"
或者 date > "2013-10-01"
date 格式是 "2013-10-01"
yea(date) ≥ 2020
给定日期"2013-10-01"，选取年月
date_format(order_date,'%Y-%m') as 'month'
left(order_date, 7)
substring(date, 1, 7)
DATEDIFF(end_date, start_date)  是 end_date - start_date
date_column +/- interval n day
# with as
with cte as (select xxx from xxx)
select * from cte;
with a as (xxx), b as (xxx)
select xxxx;
# round
round(xxx, 2) 给xx精确到小数点两位
# avg() 函数
算百分比非常有用的函数，如果统计有没有，可以和case when 1 else 0 end配合使用
# case
```sql
#简单case函数
case sex
  when '1' then '男'
  when '2' then '女’
  else '其他' end;
```
```sql
#case搜索函数
case when sex = '1' then '男'
     when sex = '2' then '女'
     else '其他' end  
```
# limit
一个非常有用的用法，当where 不能用（在出结果之后在排除某些行），用limit抛去offset
可以减少一次select 非常重要的考点
# aggregation 函数
如果是类似
select a, b from table group by a having min(c);
b 其实是随机返回的值，并不是像我们想的那样返回一对。
要用in 或者left join
同理，select a, min(b), c, d from table group by a 并不能保证c，d的一一对应关系，而是随机返回的
两个aggregation 连续用：max(count())是不能用的，所以要拆分成两个select，或者三个，用 with t, select from t where a = (select max(xx) from t) 或者 select from t where count() = select (rank limit 1)
在case 或者 if 的判断式中使用类似 max ，count， 不能直接使用，而要包含一个子查询，
eg.: (case id = (select max(id) from table) then id-1 end) id from table;
在where里直接用max也不行，必须加select子查询
当group by 使用aggregation不现实的时候，改成window函数 partition by
max的一个特殊用法：618， 始终不让一个set被数据库自动返回一个值，保持set并且可以被取值的状态。
# like
xxx like "% xxx%"
# group by
可以直接写成group by a, b
用两列group
# count(1) 就是 count(*)
# group_concat(distinct product order by product) as products
把group之后所有里面的元素连起来
# Union / Union All
把多张表格合并（多个select合并）每个表格用同样的列。
Union 会消除重复，Union All 不会
limit order by 如果作用在select里面，需要加小括号，否则就作用在union之后
在某些特殊的题目中，from 后面的东西可以把两张表先union起来在合起来用比较快
# update
update table set column = value, column2 = value2 where xxxxx
如果不加where则更新所有行
# 把字母变成ASCII，再变回字母
CHAR(ASCII('a'))
# Consecutive 类型的题目
要连续几次就把表重复join几次
# 对原来的表加一列
select column 1, column2, ...., new column from original table;
# 几个常用函数
trim
upper
lower
left
right
substring
length
concat
组合可以得到首字母大写
# 易混淆函数
sum vs  count
sum(case 1 else 0) /count(1) = avg
# Sales Analysis 系列特点
用group 后加having 条件，统计sum(待条件) 或者sum(不待条件) 并判断
# 正则表达式
1. column REGEXP '正则 '
2. regexp_like(column, '正则')
3. like "%xxx%"
# if
if(exp1, exp2, exp3) 表示 exp1 is true ? exp2 : exp3
# join ... on (xx or xx)
这是一个很神奇的用法，可以join两次
# mysql没有full join
用两次select并且union
# 窗口函数的特殊用法 window function
lead(column, offset, default) 从当前的下面
lag(column, offset, default) 从当前的上面
order by 之后的不完整偏移量用法：
rows between 1 preceding and 0 following
rows unbounded preceding
RANGE BETWEEN unbounded preceding AND CURRENT ROW
range between interval 2 day preceding and interval 2 day following
window 函数的考法非常多，另外一种考法就是和group by互换。能用上rows的就是非常难的题目了
一般考的全是rank sum count
难一点就考lead 考连续值
虽然连续两个sum不合法，但是sum(sum(amount)) over (order by visited_on rows 6 preceding)是合法的
log 用法eg：求一个人是否连续五天登陆，log(date, 4) in column 或者 date, rank() 并且count(1) > 4 再 date - rank  in column
把window 当aggragation用 sum() over (什么都不写)
# 连续序列生成
```sql
with recursive month_2020 as
(
select 1 as month union all
    select month+1 from month_2020
    where month < 12
)
select * from month_2020
```
```sql
(select 1 as month)
union (select 2 as month)
union (select 3 as month)
union (select 4 as month)
union (select 5 as month)
union (select 6 as month)
union (select 7 as month)
union (select 8 as month)
union (select 9 as month)
union (select 10 as month)
union (select 11 as month)
union (select 12 as month)
```
```sql
select months.column_0 clolumn_name FROM (
    VALUES ROW(1), ROW(2), ROW(3), ROW(4), ROW(5), ROW(6), ROW(7), ROW(8), ROW(9), ROW(10), ROW(11), ROW(12)
) AS months
```
```sql
select * from
   
    (select distinct spend_date, 'desktop' platform from spending
    union all
    select distinct spend_date, 'mobile' platform from spending
    union all
    select distinct spend_date, 'both' platform from spending) c
```
# pivot
旋转90度，mysql没有这个语句，用ifelse/case 替代
618看上去像pivot类型，但是其实他是多列合并类型的题目，并且利用了自定义变量的技巧，参考下面的经典题型和自定义变量相关内容
# recursive 用法
```sql
with recursive **cte** as (
select base from xxx
union all
select xx from **cte** where xxx
)
```
这里需要注意的是避免死循环，比如1270
```sql
with recursive cte as
(select employee_id from Employees where manager_id=1 and employee_id!=1
union all select e.employee_id from Employees e inner join cte c on e.manager_id = c.employee_id)
select * from cte
```
如果把1放入cte 则会永远跳不出循环
注意：cte引用不可以放入聚合函数，have group order limit 等等，除非在initial query中
为了解决这个问题，用inner join
如果只有一列， 用如下语法表示列名
```sql
WITH RECURSIVE cte_count (n)
AS (
      SELECT 1
      UNION ALL
      SELECT n + 1
      FROM cte_count
      WHERE n < 3
    )
SELECT n
FROM cte_count;
```
这种语法的可以有效避免distinct使用（用union而不是union all）
如果有多个cte， 需要recursive的放在前面
# 经典题型：将离散集合变成区间 1285
一串ID，把连续的id写成start | end的取件形式。
两种典型写法
```sql
select a.log_id start_id, min(b.log_id) end_id from
(select log_id from Logs where log_id - 1 not in (select log_id from Logs)) a,
(select log_id from Logs where log_id + 1 not in (select log_id from Logs)) b where a.log_id <= b.log_id
group by start_id order by start_id asc
```
```sql
select min(log_id) start_id, max(log_id) end_id from
(select log_id, log_id - (row_number() over (order by log_id asc)) dif from logs) a
group by dif order by start_id asc
```
第一种是用了一个小技巧找到边界值，然后做笛卡尔积，再钉死左边界，把找最小值为右边界
第二种是把每一行加一个行号，再做差，根据差值group （group by id - row_number）
总的来说，本质上把一列变成两列（包括变成若干列的题目，例如618）都是要**寻找各个列之间的共同点然后join 再group。**
类似的题目还有1225
```sql
SELECT period_state, MIN(days) start_date, MAX(days) end_date
FROM  
    (SELECT period_state, days,
    RANK() OVER(PARTITION BY period_state ORDER BY days) rk
    FROM
        (SELECT 'failed' period_state, fail_date days
        FROM failed
        UNION ALL
        SELECT 'succeeded' period_state, success_date days
        FROM succeeded) a
    WHERE YEAR(days) = '2019') b
GROUP BY 1, date_add(days, INTERVAL -rk DAY)
    ORDER BY 2
```
```sql
SELECT period_state, MIN(d) AS start_date, MAX(d) AS end_date
FROM (SELECT fail_date AS d, 'failed' AS period_state,
DATE_SUB(fail_date, INTERVAL ROW_NUMBER() OVER(ORDER BY fail_date) DAY) AS day
FROM Failed
WHERE fail_date BETWEEN '2019-01-01' AND '2019-12-31'
UNION ALL
SELECT success_date AS d, 'succeeded' AS period_state,
DATE_SUB(success_date, INTERVAL ROW_NUMBER() OVER(ORDER BY success_date) DAY) AS day
FROM Succeeded
WHERE success_date BETWEEN '2019-01-01' AND '2019-12-31') AS l
GROUP BY period_state, day
ORDER BY start_date
```
这个解法关注点在与如何把day 和rank结合使用求行号
我的解法复杂一点，但是用了lead 和 lag 做group依据
```sql
with a as (select ri, period_state, lag(period_state,1) over(order by ri asc) prev_ , lead(period_state,1 ) over(order by ri asc) next_ from (select fail_date ri, "failed" period_state from Failed  
union
select success_date ri, "succeeded" period_state from Succeeded) a where ri between "2019-01-01" and "2019-12-31" order by ri asc)
select b.period_state, b.start_date, c.end_date from
(select row_number() over(order by ri) rn, a.ri start_date, a.period_state from a where prev_ is null or prev_ <> period_state) b join (select row_number() over(order by ri) rn, a.ri end_date from a where next_ is null or next_ <> period_state) c using(rn) order by start_date asc
```
# 经典题型：求中位数
569
判断标准：
```sql
row_num between cnt/2.0 and cnt/2.0+1
```
```sql
RN_ASC BETWEEN RN_DESC - 1 AND RN_DESC + 1 # 两个方向各排一次
```
```sql
rnk>=total_counts/2 and rnk-1<=total_counts/2
```
```sql
SELECT
    Id, Company, Salary
FROM
    (SELECT
        e.Id,
            e.Salary,
            e.Company,
            IF(@prev = e.Company, @Rank:=@Rank + 1, @Rank:=1) AS rank,
            @prev:=e.Company
    FROM
        Employee e, (SELECT @Rank:=0, @prev:=0) AS temp
    ORDER BY e.Company , e.Salary , e.Id) Ranking
        INNER JOIN
    (SELECT
        COUNT(*) AS totalcount, Company AS name
    FROM
        Employee e2
    GROUP BY e2.Company) companycount ON companycount.name = Ranking.Company
WHERE
    Rank = FLOOR((totalcount + 1) / 2)
        OR Rank = FLOOR((totalcount + 2) / 2)
```
```sql
SELECT
    Employee.Id, Employee.Company, Employee.Salary
FROM
    Employee,
    Employee alias
WHERE
    Employee.Company = alias.Company
GROUP BY Employee.Company , Employee.Salary
HAVING SUM(CASE
    WHEN Employee.Salary = alias.Salary THEN 1
    ELSE 0
END) >= ABS(SUM(SIGN(Employee.Salary - alias.Salary)))
ORDER BY Employee.Id
;
```
571
下边界：floor((total+1)/2)
上边界：floor((total+2)/2)
```sql
WITH tb1 AS (
    SELECT *,
        -- There are cum_num numbers in TABLE numbers less than or equal to number in that record
        -- e.g. There are cum_num = 8 numbers in TABLE numbers less than or equal to 1
        -- so you will see [1,1,8,12] AS [Number, Frequency, cum_num, num]
        SUM(frequency) OVER (ORDER BY number) AS cum_num,
        SUM(frequency) OVER () AS num
    FROM numbers
)
SELECT AVG(number*1.0) AS median
FROM tb1
WHERE num / 2.0 BETWEEN cum_num - frequency AND cum_num;
```
```sql
WITH tmp AS (SELECT Number, Frequency,
             SUM(Frequency) OVER (ORDER BY Number ASC) rk1,
             SUM(Frequency) OVER (ORDER BY Number DESC) rk2
             FROM Numbers
             order by Number)
select avg(Number) as median
from tmp
where abs(rk1-rk2) <=Frequency;
```
下面是我自己的解法，练习自定义变量的
```sql
with a as (select number, frequency, sum(Frequency) over (order by number asc) ps from numbers)
select (t.u+k.l)/2 "median" from
(select min(number) u from a a, (select @down:=floor((sum(frequency)+1)/2),@upper:=floor((sum(frequency)+2)/2) from numbers) t
where a.ps >= @upper) t
join
(select min(number) l from a where a.ps >= @down) k
```
# 经典题型：把学生分成若干列：618
```sql
select min(case when continent = "America" then name end) America,
min(case when continent = "Asia" then name end) Asia,
min(case when continent = "Europe" then name end) Europe
from (select name, continent, row_number()
over(partition by continent order by name asc) nb from student) t
group by nb
```
注意这里min的用法，也可以替换成max，总之就是某一个聚类函数。这里加了聚类函数避免了group完之后只能返回一个set中的一个值，避免了null。这个max/min的用法**惊为天人 (group by id)**
或
```sql
SELECT MIN(America) America,MIN(Asia) Asia, MIN(Europe) Europe FROM
(SELECT row_number() OVER(PARTITION BY continent ORDER BY name ASC) AS n,
CASE WHEN continent='America' THEN name END AS America,
CASE WHEN continent='Asia' THEN name END AS Asia,
CASE WHEN continent='Europe' THEN name END AS Europe
FROM
student) t
GROUP BY n
```
下面是第二种解法，用自定义变量
```sql
select max(America) America, max(Asia) Asia, max(Europe) Europe from
(select
if(s.continent = 'America', name, null) as America,
if(s.continent = 'Europe', name, null) as Europe,
if(s.continent = 'Asia',name ,null) as Asia,
(case when s.continent = 'America' then @r1:= @r1+1
      when s.continent = 'Europe' then @r2:= @r2+1
      when s.continent = 'Asia' then  @r3:= @r3+1 end) as row_id
from student s, (select @r1:= 0, @r2:= 0, @r3:= 0) t
order by name) m
group by row_id order by row_id
```
此处还是和之前的解法相似，注意自定义变量的用法
# 自定义变量
基本模式：把变量结合到表中
```sql
select * , (@var := @var + 1) "name"
from Table a, (select @var := 0) t
```
定义变量用
```sql
select @var1 := 0, @var2 := 0 ....
```
如果是命令行中定义用set，既可以是：=也可以是=。select中只允许：=
：= 的优先级是所有语句中最低的，所以要加上括号
注意刚select是不能列出表的，必须select两次才能显式看到表格
另一种变量@@var注意不能用在此处
大小写不敏感
未定义的就是null
用在where和having中必须另写一个select

## SQL效率问题

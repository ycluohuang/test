# Sql指令大全



1. 

```
select * from student;
```

获取学生表中的所有学生信息

   2.**选择查询**

```
select name, gender from students;
```

使用"选择查询"来获取所有学生的姓名（name）和性别（gender）信息

3. **别名**

```
select name as 员工姓名, position as 职位名称 from employees;
```

使用 "别名" 来获取所有团队成员的姓名 name和职位 position信息，并为它们取别名为 `员工姓名` 和 `职位名称`：

4.    **查询表可以直接使用四则运算**

5.  **筛选查询**

   使用 "WHERE" 来筛选出库存小于等于 20 的产品：

   ```
   select name, price, stock from products where stock <= 20;
   ```

6. 使用 "IS NULL" 来查询出入职日期未填写的员工：

   ```
   select name, age from employees where hire_date is null;
   ```

7. **模糊查询**

   使用 LIKE 模糊查询来找出姓名（name）中包含关键字 "张" 的员工信息：

   还可以使用模糊查询匹配开头和结尾：

   同理，可以使用 `not like` 来查询不包含某关键字的信息。

   ```
   select name, age, position from employees where name like '%张%';
   
   -- 只查询以 "张" 开头的数据行
   select name, age, position from employees where name like '张%';
   
   -- 只查询以 "张" 结尾的数据行
   select name, age, position from employees where name like '%张';
   ```

8.  **逻辑运算**

   在逻辑运算中，常用的运算符有：

   - AND：表示逻辑与，要求同时满足多个条件，才返回 true。
   - OR：表示逻辑或，要求满足其中任意一个条件，就返回 true。
   - NOT：表示逻辑非，用于否定一个条件（本来是 true，用了 not 后转为 false）

   我们使用逻辑运算来找出姓名中包含关键字 "李" **且** 年龄小于 30 岁的员工信息：

   ```
   select name, age, salary from employees where name like '%李%' and age < 30;
   ```

9.  **去重**

​		我们使用`DISTINCT`关键字来找出不同的班级 ID：

```
select distinct class_id from students;   //找不重复的
```

​	10. **排序**

```
-- SQL 查询语句 1
select name, age from students order by age asc; (ascend)

-- SQL 查询语句 2
select name, score from students order by score desc;(desascend)
```

在排序的基础上，我们还可以根据多个字段的值进行排序。当第一个字段的值相同时，再按照第二个字段的值进行排序，以此类推。

```
order by 字段1 [升序/降序], 字段2 [升序/降序], ...
```

11.**截断和偏移**

1）你可以使用手指挡住不需要看的部分（即截断）

2）根据任务的编号，直接翻到需要查看的位置（即偏移）

在 SQL 中，我们使用 `LIMIT` 关键字来实现数据的截断和偏移。

截断和偏移的一个典型的应用场景是分页，即网站内容很多时，用户可以根据页号每次只看部分数据。

```
-- LIMIT 后只跟一个整数，表示要截断的数据条数（一次获取几条）
select task_name, due_date from tasks limit 2;

-- LIMIT 后跟 2 个整数，依次表示从第几条数据开始、一次获取几条
select task_name, due_date from tasks limit 2, 2;  // 第一个指的是下标,从0开始
```

12. **条件分支**

    使用条件分支 `case when` ，根据 name 来判断学生是否会说 RAP，并起别名为 can_rap。

    ```
    SELECT
      name,
      CASE WHEN (name = '鸡哥') THEN '会' ELSE '不会' END AS can_rap
    FROM
      student;
    ```

    查询结果：

    | name | can_rap |
    | ---- | :-----: |
    | 小明 |  不会   |
    | 鸡哥 |   会    |
    | 李华 |  不会   |
    | 王五 |  不会   |

​		`case when` 支持同时指定多个分支，示例语法如下：	

```sql
CASE WHEN (条件1) THEN 结果1
	   WHEN (条件2) THEN 结果2
	   ...
	   ELSE 其他结果 END
```

##### 实例：

假设有一个学生表 `student`，包含以下字段：`name`（姓名）、`age`（年龄）。请你编写一个 SQL 查询，将学生按照年龄划分为三个年龄等级（age_level）：60 岁以上为 "老同学"，20 岁以上（不包括 60 岁以上）为 "年轻"，20 岁及以下、以及没有年龄信息为 "小同学"。

返回结果应包含学生的姓名（name）和年龄等级（age_level），并按姓名升序排序。

```sql
select 
    name,
    case when(age > 60) then "老同学"
        when(age > 20) then "年轻"
        else "小同学"
        END as age_level
from student order by name asc
```

13. **时间函数**

    常用的时间函数有：

    - DATE：获取当前日期

    - DATETIME：获取当前日期时间

    - TIME：获取当前时间

      ```
      -- 获取当前日期
      SELECT DATE() AS current_date;
      
      -- 获取当前日期时间
      SELECT DATETIME() AS current_datetime;
      
      -- 获取当前时间
      SELECT TIME() AS current_time;
      ```

14.    **字符串处理**

使用字符串处理函数 `UPPER` 将姓名转换为大写：

使用字符串处理函数 `LOWER` 将姓名转换为小写：

```
-- 将姓名转换为大写
SELECT name, UPPER(name) AS upper_name
FROM employees;

-- 将姓名转换为小写并进行条件筛选
SELECT name, LOWER(name) AS lower_name
FROM employees;
```

使用字符串处理函数 `LENGTH` 计算姓名长度：

```
-- 计算姓名长度
SELECT name, LENGTH(name) AS name_length
FROM employees;
```

15.  **聚合函数**

常见的聚合函数包括：

- COUNT：计算指定列的行数或非空值的数量。
- SUM：计算指定列的数值之和。
- AVG：计算指定列的数值平均值。
- MAX：找出指定列的最大值。
- MIN：找出指定列的最小值。

使用聚合函数 `COUNT` 计算订单表中的总订单数：

```
SELECT COUNT(*) AS order_num  FROM orders;
```

使用聚合函数 `COUNT(DISTINCT 列名)` 计算订单表中不同客户的数量：

```
SELECT COUNT(DISTINCT customer_id) AS customer_num  FROM orders;
```

使用聚合函数 `SUM` 计算总订单金额：

```
SELECT SUM(amount) AS total_amount FROM orders;
```

16.    **单字段分组**

    使用分组聚合查询中每个客户的编号：

    ```
    SELECT customer_id
    FROM orders
    GROUP BY customer_id;
    ```

    使用分组聚合查询每个客户的下单数：

    ```
    SELECT customer_id, COUNT(order_id) AS order_num
    FROM orders
    GROUP BY customer_id;
    ```

    查询结果：

    | customer_id | order_num |
    | :---------: | --------- |
    |    A001     | 2         |
    |    A002     | 1         |
    |    A003     | 1         |

​         	**多字段分组**

​	要查询使用多字段分组查询表中 **每个客户** 购买的 **每种商品** 的总金额，相当于按照客户编号和商品编号分组：

```
SELECT customer_id, product_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id, product_id;
```

​	17.     **分组聚合 - having 子句**

​	HAVING 子句与条件查询 WHERE 子句的区别在于，WHERE 子句用于在 **分组之前** 进行过滤，而 HAVING 子句用于在 **分组之后** 进行过滤。

使用 HAVING 子句查询订单数超过 1 的客户：

```
SELECT customer_id, COUNT(order_id) AS order_num
FROM orders
GROUP BY customer_id
HAVING COUNT(order_id) > 1;
```

使用 HAVING 子句查询订单总金额超过 100 的客户：

```
SELECT customer_id, SUM(amount) AS total_amount
FROM orders
GROUP BY customer_id
HAVING SUM(amount) > 100;
```

 



### 进阶查询

18. ​     **关联查询 - cross join**

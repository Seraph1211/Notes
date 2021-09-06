# 1、SQL简介

SQL （Structured Query Language, 结构化查询语言）用于管理关系型数据库系统（RDBMS）。SQL的作用范围包括数据插入、查询、更新和删除，数据库模式创建和修改，以及数据访问控制。

SQL的功能：

- SQL 面向数据库执行查询
- SQL 可从数据库取回数据
- SQL 可在数据库中插入新的记录
- SQL 可更新数据库中的数据
- SQL 可从数据库删除记录
- SQL 可创建新数据库
- SQL 可在数据库中创建新表
- SQL 可在数据库中创建存储过程
- SQL 可在数据库中创建视图
- SQL 可以设置表、存储过程和视图的权限



# 2、SQL语句

一些重要的SQL命令：

- **SELECT** - 从数据库中提取数据
- **UPDATE** - 更新数据库中的数据
- **DELETE** - 从数据库中删除数据
- **INSERT INTO** - 向数据库中插入新数据
- **CREATE DATABASE** - 创建新数据库
- **ALTER DATABASE** - 修改数据库
- **CREATE TABLE** - 创建新表
- **ALTER TABLE** - 变更（改变）数据库表
- **DROP TABLE** - 删除表
- **CREATE INDEX** - 创建索引（搜索键）
- **DROP INDEX** - 删除索引

**注意：**

1. SQL对大小写不敏感，SELECT和select效果是一样的。
2. 每条SQL语句末端要加分号。

## 2.1  SELECT语句

SELECT语句用于从数据库中选取数据。结果被存储在一个结果表中，称为结果集。

SELECT语法如下：

```sql
SELECT column_name, column_name
FROM table_name;
```

或者

```sql
select * from table_name;
```

前者表示从表中选取一个或多个特定的列，后者表示获取表中所有的列。

## 2.2  SELECT DISTINCT语句

在表中，一个列可能会含有多个重复的值，DISTINCT关键词用于返回唯一不同的值（即去掉重复值）。

SELECT DISTINCT语法如下：

```sql
SELECT DISTINCT column_name, column_name
FROM table_name;
```

## 2.3  WHERE子句

WHERE子句用于提取满足指定条件的记录。

WHERE语法：

```sql
SELECT column_name, column_name
FROM table_name
WHERE column_name operator value;
```

注意：SQL使用单引号来环绕文本字段，数值字段不需要使用引号。

```sql
select * from student_grades
where subject = '数学';
```

#### WHERE子句中的运算符

以下运算符可以在where子句中使用。

| 运算符  | 描述                                                       |
| :------ | :--------------------------------------------------------- |
| =       | 等于                                                       |
| <>      | 不等于。**注释：**在 SQL 的一些版本中，该操作符可被写成 != |
| >       | 大于                                                       |
| <       | 小于                                                       |
| >=      | 大于等于                                                   |
| <=      | 小于等于                                                   |
| BETWEEN | 在某个范围内                                               |
| LIKE    | 搜索某种模式                                               |
| IN      | 指定针对某个列的多个可能值                                 |



## 2.4  AND & OR 运算符

AND & OR 运算符用于基于一个以上的条件记录进行过滤。

* AND运算符：返回满足AND前后两个条件的记录。
* OR运算符：返回满足OR前后两个条件中任意一个条件的记录。

AND运算符实例：

```sql
select * from Websites
where country = 'CN'
and alexa > 50;
```

OR运算符实例：

```sql
SELECT * FROM Websites
WHERE country = 'CN'
OR country = 'USA';
```

AND & OR 的结合使用：

```sql
SELECT * FROM Websites
WHERE alexa > 50
AND (country = 'CN' OR country = 'USA');
```



## 2.5  ORDER BY 关键字

ORDER BY 关键字用于将结果集按照某一列或多列进行排序。

* ASC：升序排序。默认排序方式。
* DESC：降序排序。

ORDER BY 语法：

```sql
SELECT column_name, column_name
FROM table_name
ORDER BY column_name, column_name ASC|DESC;
```

当对多列进行排序时，按照所要排序的列在SQL语句中的先后位置，先后进行排序。



## 2.6  INSERT INTO 语句

INSERT INTO 语句用于向表中插入新纪录。

INSERT INTO 语法格式如下：

```sql
INSERT INTO table_name
VALUES (value1, value2, value3, ...);
```

或者

```sql
INSERT INTO table_name(column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
```

前者无需指定要插入的列名，只需提供被插入的值；后者需要指定列名并提供被插入的值。



## 2.7  UPDATE 语句

UPDATE 语句用于更新表中的记录。

UPDATE 语法格式如下：

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE some_column = some_value;
```

上述Sql语句中的where子句用于指定那些记录需要更新，如果省略where子句，则所有记录都将被更新。



## 2.8  DELETE 语句

DELETE 语句用于删除表中的行。

语法如下：

```sql
DELETE FROM table_name
WHERE some_column = some_value;
```

WHERE 子句规定哪条记录或者哪些记录需要删除。如果省略了 WHERE 子句，所有的记录都将被删除。



## 2.9  SELECT LIMIT子句

SELECT  LIMIT子句用于规定要返回的记录数目。

语法格式如下：

```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

在SQL Server中还可以使用百分比作为参数。（以上是MySql的用法）



## 2.10  LIKE操作符

LIKE操作符用于在WHERE子句中搜索列中的指定模式。（模糊搜索）

语法如下：

```sql
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern;
```

如在Website表中搜索name字段以字母"G"开头的所有记录：

```sql
select * from Website
where name like 'G%';
```

在Website表中搜索name字段以字母"k"结尾的所有记录：

```sql
select * from Website
where name like '%k';
```

在Website表中搜索name字段包含"oo"的所有记录：

```sql
select * from Website
where name like '%oo%';
```



## 2.11  通配符

通配符可用于替代字符串中的任何其他字符，一般与LIKE操作符配合使用。

SQL中有以下通配符：

| 通配符                         | 描述                       |
| :----------------------------- | :------------------------- |
| %                              | 替代 0 个或多个字符        |
| _                              | 替代一个字符               |
| [*charlist*]                   | 字符列中的任何单一字符     |
| [^*charlist*] 或 [!*charlist*] | 不在字符列中的任何单一字符 |

MySQL中以**REGEXP**或**NOT REGEXP**运算符（或**RLIKE** 和 NOT RLIKE）来操作正则表达式。

如从Website表中选取name字段以'G', 'F' 或 's' 开头的所有记录：

```sql
select * from Website
where name regexp '^[GFs]';
```

从Website表中选取name字段以'A' - 'H' 开头的所有记录：

 ```sql
select * from Website
where name regexp '^[A-H]';
 ```

从Website表中选取name字段不以'A' - 'H' 开头的所有记录：

```sql
select * from Website
where name regexp '^[!A-H]';
```



## 2.12  IN操作符

IN操作符可以在WHERE子句中规定多个值。

语法如下：

```sql
select column_name(s)
from table_name
where column_name in (value1, value2, value3, ...);
```



## 2.13  BETWEEN操作符

BETWEEN操作符用于选取介于两个值（数值、文本、日期）之间的数据。

语法如下：

```sql
select column_name(s)
from table_name
where column_name (not) between value1 and value2;
```

所指定的数据范围为[value1, value2].

BETWEEN操作符可以和IN操作符配合使用：

```sql
select * from Website
where (alexa between 1 and 20) and country not in ('USA', 'CN');
```



## 2.14  别名

我们可以为表或者列指定别名。创建别名的目的是为了让表或者列名称的可读性更强。

为列创建别名的语法如下：

```sql
SELECT column_name AS alias_name
from table_name;
```

为表创建别名的语法如下：

```sql
SELECT column_name(s)
FROM table_name AS alias_name;
```

在以下情况下，别名很有用：

* 查询中涉及超过一个表。
* 在查询中使用了函数。
* 列或表的名称很长或可读性很差。
* 需要把多个列结合在一起。



## 2.15  连接(JOIN)

join用于把两个或多个表的行结合起来。

### 2.15.1  LEFT JOIN

从左表(table1)返回所有的行，即使右表(table2)没有匹配。如果右表没有匹配，则结果为null。

语法如下：

```sql
select column_name(s)
from table1
left join table2
on table1.column_name = table2.column_name;
```

![SQL LEFT JOIN](https://www.runoob.com/wp-content/uploads/2013/09/img_leftjoin.gif)

![](https://www.runoob.com/wp-content/uploads/2013/09/left-join1.jpg)

### 2.15.2  RIGHT JOIN 

从右表(table2)返回所有的行，即使左表(table1)中没有匹配。如果左表中没有匹配，则结果为null。

语法如下：

```sql
select column_name(s)
from table1
right join table2
on table1.column_name = table2.column_name;
```

![](https://www.runoob.com/wp-content/uploads/2013/09/img_rightjoin.gif)

![](https://www.runoob.com/wp-content/uploads/2013/09/402A662D-3553-449C-B980-942D864412BD.jpg)

### 2.15.3  INNER JOIN

在表中存在至少一个匹配时返回行。

语法如下：

```sql
select column_name(s)
from table1
inner join table2
on table1.column_name = table2.column_name;
```

或

```sql
select column_name(s)
from table1
join table2
on table1.column_name = table2.column_name;
```

inner join 和 join效果是一样的。

![SQL INNER JOIN](https://www.runoob.com/wp-content/uploads/2013/09/img_innerjoin.gif)



### 2.15.4  OUTER JOIN 

只要左表(table1)或右表(table2)中只要有一个存在匹配，则返回行。

语法如下：

```sql
select column_names(s)
from table1
outer join table2
on table1.column_name = table2.column_name;
```

### 2.15.5  总结

* A inner join B：取交集。
* A left join B：取A的全部，B没有对应的值即为null。
* A right join B：取B的全部，A没有对应的值即为null。
* A outer join B：取并集，彼此没有对应的值即取null。



## 2.16  UNION操作符

合并两个或多个select语句的结果集。

语法如下：

1. union：默认选取所有不同的值

```sql
select column_name(s) from table1
union
select column_name(s) from table2;
```

2. union all：允许重复的值  

```sql
select column_name(s) from table1
union all
select column_name(s) from table2;
```

要求union内部的每个select语句必须拥有相同数量的列，列也必须拥有相似的数据类型。同时，每个select语句中列的顺序也必须相同。



## 2.17  SELECT INTO语句

从一个表中复制数据，然后把数据插入到另一新表中。

**MySql数据库不支持该语法！**

语法如下：

```sql
select * 
into newtable
from table1;
```



## 2.18  INSERT INTO SELECT 语句

从一个表中复制数据并插入到一个已存在的表中。

语法如下：

```sql
insert into table2
select * from table1;
```



## 2.19  CREATE DATABASE语句

用于创建数据库。

语法如下：

```sql
create database dbname;
```



## 2.20  CREATE TABLE语句

用于创建数据库中的表。

语法如下：

```sql
create table table_name
(
column_name1 data_type(size),
column_name2 data_type(size),
column_name3 data_type(size),
...
)
```



## 2.21  约束(Constraints)

约束用于规定表中的数据规则。

如果存在违反约束的数据行为，行为会被约束终止。

建表前后都可以设置约束。



### 2.21.1  NOT NULL

某列不能存储null值。强制约束某字段始终包含值，即如果不向该字段添加值，就无法插入新纪录。

![image-20210529162254970](C:\Users\Seraph\AppData\Roaming\Typora\typora-user-images\image-20210529162254970.png)

### 2.21.2  UNIQUE

保证某列的每行必须有唯一的值。为列或列集合提供了唯一性的保证。

一个表可以有多个UNIQUE约束。

语法格式如下：

1. 建表时创建Unique约束：

```sql
create table Person
(
P_Id int not null,
...
unique (P_Id)
)
```

2. 建表时创建并命名unique约束：

```sql
create table Persoon
(
    P_Id int not null,
    LastName varchar(255) not null,
    ...
    constraint u_PId unique (P_Id, LastName)
)
```

3. 建表后创建UNIQUE约束：

```sql
alter table Person
add unique (P_Id);
```

4. 撤销UNIQUE约束：

```sql
alter table Person
drop index u_PId;
```





### 2.21.3  PRIMARY KEY

NOT NULL 和 UNIQUE的结合。确保某列（或多个列的结合）有唯一标识，有助于快速找到表中的一个特定的记录。



### 2.21.4  FOREIGN KEY

保证一个表中的数据匹配另一个表中值的完整参照性。



### 2.21.5  CHECK  

保证列中数据的值符合指定的条件。



### 2.21.6  DEFAULT

规定没有给列赋值时的默认值。
















### DML语言

> 数据操作语言
>
> 插入：insert
>
> 修改：update
>
> 删除：delete

#### 插入语句

##### 方式一

> 语法
>
> insert into 表名(列名,...)
>
> values(值1,...)

1、插入的值的类型要和列的类型一致或兼容

```mysql
INSERT INTO beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'唐艺昕','女','1990-04-23','18988888888',NULL,2);
```

2、不可以为NULL的列必须插入值，可以为NULL的列是如何插入值的？

```mysql
方式一：将对应的位置设置为NULL
INSERT INTO beauty(id,name,sex,borndate,phone,photo,boyfriend_id)
VALUES(13,'唐艺昕','女','1990-04-23','18988888888',NULL,2);

方式二：插入的时候对应的列不进行插入
INSERT INTO beauty(id,name,sex,borndate,phone,boyfriend_id)
VALUES(13,'唐艺昕','女','1990-04-23','18988888888',2);
```

3、列的顺序是否可以调换

```mysql
INSERT INTO beauty(name,sex,id,phone)
VALUES('蒋欣','女',16,'110');
```

4、列数和值的个数必须一致

5、可以省略列名，默认是所有列，而且列的顺序和表中列的顺序一致

```mysql
INSERT INTO beauty
VALUES(18,'张飞','男',NULL,'119',NULL,NULL);
```

##### 方式二

> 语法
>
> inset into 表名
>
> set 列名=值,列名=值,...

```mysql
INSERT INTO beauty
SET id=19,name='刘涛',phone='999';
```

**两种方式PK**

1、方式一支持插入多行，方式二不支持

```mysql
INSERT INTO beauty
VALUES(13,'唐艺昕','女','1990-04-23','18988888888',2),(14,'刘涛','女','1993-04-23','99999999',3);
```

2、方式一支持子查询，方式二不支持

```mysql
INSERT INTO beauty(id,name,phone)
SELECT 26,'宋茜','123123';
```

#### 修改语句

> 1、修改单表的记录
>
> 语法：
>
> update 表名
>
> set 列 = 新值,列 = 新列,...
>
> where 筛选条件;
>
> 2、修改多表的记录【补充】
>
> 
>
> sql92标准
>
> update 表1 别名,表2 别名
>
> set 列=值,...
>
> where 连接条件
>
> and 筛选条件
>
> 
>
> sql99标准
>
> update 表1 别名
>
> inner | left | right  join 表2 别名
>
> on 连接条件
>
> set 列=值,...
>
> where 筛选条件

1、修改beauty表中姓唐的电话改为 123456

```mysql
UPDATE beauty SET phone = '123456' WHERE name LIKE '唐%'
```

2、修改boys表中id号为2的名称为张飞，魅力值为10

```mysql
UPDATE boys set boyname = '张飞',userCP = 1000 WHERE id = 2;
```

3、修改张无忌的女朋友的手机号为114

```mysql
UPDATE 
boys bo INNER JOIN beauty b
ON bo.id = b.boyfriend_id
SET b.phone = '114'
WHERE bo.boyname = '张无忌';
```

4、修改没有男朋友的女神的男朋友编号都为2号

```mysql
UPDATE boys bo
RIGHT JOIN beauty b ON bo.id = b.boyfriend_id
SET b.boyfriend_id = 2
WHERE bo.id IS NULL
```

#### 删除语句

##### 方式一

> 关键字：delete（一删除就是删除整行）
>
> 1、单表的删除
>
> 语法：delete from 表名 where 筛选条件
>
> 2、多表的删除【补充】
>
> 
>
> sql92标准
>
> delete 表1别名,表2别名
>
> from 表1 别名,表2 别名
>
> where 连接条件
>
> and 筛选条件
>
> 
>
> sql99标准
>
> delete 表1的别名,表2的别名
>
> from 表1 别名
>
> inner | left | right join 表2 别名 
>
> on 连接条件
>
> where 筛选条件



1、删除手机号以9结尾的女神信息（单表删除）

```mysql
DELETE FROM beauty WHERE phone LIKE '%9';
```

2、删除张无忌的女朋友信息

```mysql
DELETE b
FROM beauty b
INNER JOIN boys bo ON b.boyfriend_id = bo.id
WHERE bo.boyname = '张无忌';
```

3、删除黄晓明的信息以及他女朋友的信息

```mysql
DELETE b,bo
FROM beauty b
INNER JOIN boys bo ON b.boyfriend_id = bo.id
WHERE bo.boyname = '黄晓明';
```

##### 方式二

> 关键字：truncate（这个删除就是将整个表中的数据进行删除）
>
> 语法：truncate table 表名

1、将魅力值大于100的男神信息删除

```mysql
TRUNCATE TABLE boys
```

**delete pk truncate**

1、delete 可以加 where 条件，truncate 不能加

2、truncate 删除相对于 delete 高一些

3、加入要删除的表中有自增长列，如果使用delete删除后，再插入数据，自增长列的值从断点开始，而truncate删除后，再插入数据，自增长列的值从1开始

4、truncate删除没有返回值，delete删除存在有返回值

5、truncate删除不能回滚，delete删除可以回滚






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

### DDL语言

> 数据定义语言
>
> 库和表的管理
>
> 一、库的管理
>
> 创建、修改、删除
>
> 二、表的管理
>
> 创建、修改、删除
>
> 
>
> 创建：create
>
> 修改：alter
>
> 删除：drop

#### 库的管理

##### 库的创建

> 语法：create database 库名;

```CREATE DATABASE books``` 这样的方式对于不存在的库可能正常执行，但是如果存在对应的表则会报错，这个时候可以使用 ```CREATE DATABASE IF NOT EXISTS books```

##### 库的修改

> 一般情况下是不进行修改的

```RENAME DATABASE books TO 新库名``` 该语句在之前还能够使用，在现在已经废弃掉了

**更改库的字符集**

```ALTER DATABASE books CHARACTER SET gbk;```

##### 库的删除

```DROP DATABASE books``` 如果这个库已经删除了，使用这个sql则会报错，可以使用```DROP DATABASE IF EXISTS books```

#### 表的管理

##### 表的创建

> 语法：
>
> create table 表名 (
>
> ​	列名 列的类型[(长度) 约束],
>
> ​	列名 列的类型[(长度) 约束],
>
> ​	......
>
> )

1、创建表books

```
CREATE TABLE books(
  id INT, # 书的编号
  b_name VARCHAR(20),  # 书名
  price DOUBLE,  # 价格
  author VARCHAR(20),  # 作者
  publish_data DATETIME  # 出版日期
)
```

2、创建表author

```mysql
CREATE TABLE author(
  id INT,
  au_name VARCHAR(20),
  nation VARCHAR(10)
)
```

##### 表的修改

> alter table 表名 add/drop/modify/change column 列名 [列类型 约束]

1、修改列名

```mysql
ALTER TABLE books CHANGE COLUMN publish_date pubDate DATETIME;
```

2、修改列的类型或约束

```mysql
ALTER TABLE books MODIFY COLUMN pubDate TIMESTAMP;
```

3、添加新列

```mysql
ALTER TABLE books ADD COLUMN annual DOUBLE;
```

4、删除列

```mysql
ALTER TABLE books DROP COLUMN annual DOUBLE;
```

5、修改表名

```mysql
ALTER TABLE books RENAME TO books_s
```

##### 表的删除

```mysql
DROP TABLE author;
```

##### 表的复制

1、仅仅复制表的结构

```mysql
CREATE TABLE stuinfo_copy LIKE stuinfo;
```

2、复制表的结构 + 全部数据

```mysql
CREATE TABLE stuinfo_copy_2 SELECT * FROM stuinfo;
```

3、只复制部分数据

```mysql
CREATE TABLE stuinfo_copy3 
SELECT id,au_name 
FROM stuinfo 
WHERE nation = '中国';
```

4、仅仅复制某些字段

```mysql
CREATE TABLE stuinfo_copy3
SELECT id,au_name
FROM author
WHERE 1=2;
```

### 常见的数据类型

> 数值型
>
> - 整型
> - 小数
>   - 定点数
>   - 浮点数
>
> 字符型
>
> - 较短的文本
>   - char、varchar
> - 较长的文本
>   - text、blob（较长的二进制数据）
>
> 日期型

#### 整型

| 整数类型    | 字节 | 范围                                  |
| ----------- | ---- | ------------------------------------- |
| tinyint     | 1    | 有符号：-128～127；无符号0～255       |
| smallint    | 2    | 有符号：-32768～32768；无符号0～65535 |
| mediumint   | 3    | 很大                                  |
| int/integer | 4    | 很大                                  |
| bigint      | 8    | 很大                                  |

> 特点：
>
> 1、如果不设置无符号或有符号，默认是有符号；如果想要设置无符号，需要添加 UNSIGNED 关键字
>
> 2、如果插入的数值超出了整型的范围，会报out of range 异常，并且插入的是临界值
>
> 3、如果不设置长度，会有默认的长度
>
> 4、在设置类型时括号中的长度代表了显示的组大宽度，如果不够会用0在左边填充（必须搭配zerefill使用），例如int(7)并不是表示最大只能支持7的长度，而是代表的显示的长度

1、如何设置无符号和有符号

```mysql
CREATE TABLE tab_int(
  t1 INT
  t2 INT UNSIGNED  # 这里设置的t2字段就是无符号的
)
```

#### 小数

| 浮点数类型 | 字节 | 范围                                                      |
| ---------- | ---- | --------------------------------------------------------- |
| float      | 4    | 很大                                                      |
| double     | 8    | 很大                                                      |
| 定点数类型 | 字节 | 范围                                                      |
| DEC(M,D)/  | M+2  | 最大取值和double相同，给定decimal的有效取值范围由M和D决定 |

> 1、浮点型
>
> - float(M,D)
> - double(M,D)
>
> 2、定点型
>
> - dec(M,D)
> - decimal(M,D)
>
> 特点：
>
> 1、M和D
>
> M：整数部位 + 小数部位
>
> D：小数部位
>
> 如果查过范围，则插入临界值0
>
> 2、M和D都可以省略，如果是decimal，则M默认是10，D默认是0；如果是float或double，则会根据插入的数值的精度来决定精度
>
> 3、定点型的精度较高，如果要求插入数值的精度较高如货币运算则考虑使用

```mysql
CREATE TABLE tab_float(
	f1 FLOAT(5,2),
	f2 DOUBLE(5,2),
	f3 DECIMAL(5,2)
);
通过上面的方式就能够创建小数类型的表
```

***原则：所选择的类型越简单越好，能保证数值的类型越小越好***

#### 字符型

> 较短的文本：
>
> char
>
> varchar
>
> 其他：
>
> binary和varbinary用于保存较短的二进制
>
> enum用于保存枚举
>
> set用于保存集合
>
> 较长的文本：
>
> text
>
> blob(较大的二进制)

1、char和varchar类型（用来保存mysql中较短的字符串）

| 字符串类型 | 最多字符数           | 描述及存储需求        | 特点           | 空间的耗费 | 效率 |
| ---------- | -------------------- | --------------------- | -------------- | ---------- | ---- |
| char(M)    | M，可以省略，默认为1 | M为0～255之间的整数   | 固定长度的字符 | 比较耗费   | 高   |
| varchar(M) | M，不可以省略        | M为0～65535之间的整数 | 可变长度的字符 | 比较节省   | 低   |

***注意：这里指的是字符数，并不是字节数；一个字母a是一个字符，一个中文汉字也是一个字符***

#### 日期

| 日期和时间格式 | 字节 | 最小值              | 最大值              |
| -------------- | ---- | ------------------- | ------------------- |
| date           | 4    | 1000-01-01          | 9999-12-31          |
| datetime       | 8    | 1000-01-01 00:00:00 | 9999-12-31 23:59:59 |
| timestamp      | 4    | 19700101080001      | 2038年的某个时刻    |
| time           | 3    | -838:59:59          | 838:59:59           |
| year           | 1    | 1901                | 2155                |

todo




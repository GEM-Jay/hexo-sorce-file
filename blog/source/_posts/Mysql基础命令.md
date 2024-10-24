---
title: Mysql基础命令
topic: id
tags:
  - 技术
  - Mysql
categories:
  - 技术
repo: GEM-Jay/GEM-Jay.github.io
abbrlink: 4265605807
date: 2024-10-16 13:24:44
---

# mysql基基础——常用命令

## 一、MySQL——常用命令

### 1、创建数据库（Create）

```命令
mysql> create database db_name;  -- 创建数据库
mysql> show databases;           -- 显示所有的数据库
mysql> drop database db_name;    -- 删除数据库
mysql> use db_name;              -- 选择数据库
mysql> create table tb_name (字段名 varchar(20), 字段名 char(1));   -- 创建数据表模板
mysql> show tables;              -- 显示数据表
mysql> desc tb_name；            -- 显示表结构
mysql> drop table tb_name；      -- 删除表
```

例如：  
创建学生表

```例子
create table Student(
  Sno char(10) primary key,
  Sname char(20) unique,
  Ssex char(2),
  Sage smallint,
  Sdept char(20)
)；
```

### 2、插入数据（Insert）

insert into 语句用于向表格中插入新的行:

```命令
/*第一种形式无需指定要插入数据的列名，只需提供被插入的值即可：*/
mysql> insert into tb_name values (value1,value2,value3,...);
/*第二种形式需要指定列名及被插入的值：*/
mysql> insert into tb_name (column1,column2,column3,...) values (value1,value2,value3,...);
```

例如：  
插入数据

```例子
mysql> insert into Student values ( 20180001,张三,男,20,CS）;
mysql> insert into Student values ( 20180002,李四,男,19,CS）;
mysql> insert into Student (Sno,Sname,Ssex,Sage,Sdept) values ( 20180003,王五,男,18,MA）;
mysql> insert into Student (Sno,Sname,Ssex,Sage,Sdept) values ( 20180004,赵六,男,20,IS）;
```

### 3、查询数据（Select）

select语句除了可以查看数据库中的表格和视图的信息外,还可以查看 SQL Server的系统信息、复制、创建数据表。其查询功能强大，是SQL语言的灵魂语句，也是SQL中使用频率最高的语句。

基本select语句：  
一个基本的select语句可分解成三个部分：查找什么数据（select）、从哪里查找（from）、查找的条件是什么（where）。

select 语句的一般格式如下：

```命令
select <目标列表达式列表>
[into 新表名]
from 表名或视图名
[where <条件>]
[group by <分组表达式>]
[having <条件>]
[order by <排序表达式>[ASC|DESC]]
```

#### \(一\)查询指定的列

##### 1.查询表中所有列

在select语句指定列的位置上使用\*号时，表示查询表的所有列。

```命令
select * from tb_name
```

##### 2.查询表中指定的列

查询多列时，列名之间要用逗号隔开。

```命令
select tb_name.<字符型字段>,<字符型字段>... from  tb_name;
```

##### 3.指定查询结果中的列标题

通过指定列标题（也叫列别名）可使输出结果更容易被人理解。  
指定列标题时，可在列名之后使用AS子句；也可使用:列别名=\<表达式>的形式指定列标题。

AS子句的格式为：列名或计算表达式 \[AS\] 列标题

```命令
select <字符型字段> as 列标题1,<字符型字段> as 列标题2, <字符型字段> as 列标题3 from bt_name；
```

##### 4.查询经过计算的列（即表达式的值）

使用select对列进行查询时，不仅可以直接以列的原始值作为结果，而且还可以将列值进行计算后所得值作为查询结果，即select子句可以查询表达式的值,表达式可由列名、常量及算术运算符组成。  
查询结果计算列显示“无列名”,一般要给计算列加列标题。  
其中：表达式中可以使用的运算符有：加+、减-、乘\*、除/、取余\%

```命令
select <字符型字段>,<字符型字段>,列标题 = <字符型字段> * n from tb_name；
```

#### \(二\)选择行：选择表中的部分行或全部行作为查询的结果

```命令
select [all|distinct] [top n[percent]]<目标列表达式列表> from 表名
```

##### 1\. 消除查询结果中的重复行

对于关系数据库来说，表中的每一行都必须是不同的\(即无重复行\)。但当对表进行查询时若只选择其中的某些列，查询结果中就可能会出现重复行。  
在select语句中使用distinct关键字可以消除结果集中的重复行

```命令
select distinct <字符型字段>[,<字符型字段>,...] from tb_name；
```

##### 2\. 限制查询结果中的返回行数

使用top选项可限制查询结果的返回行数，即返回指定个数的记录数。  
其中：n是一个正整数，表示返回查询结果集的前n行；若带percent关键字，则表示返回结果集的前n\%行。

```命令
celect  top n from tb_name; /*查询前 n 的数据*/
celect top n percent from tb_name; /*查询前 n% tb_name的数据*/
```

#### \(三\)查询满足条件的行: 用where子句实现条件查询

通过where子句实现,该子句必须紧跟在From子句之后

```命令
select [all|distinct] [top n[percent]]<目标列表达式列表> from 表名 where <条件>；
```

说明：在查询条件中可使用以下运算符或表达式  
运算符                 运算符标识  
比较运算符          \<=，\<，=，>，>=，\!=，\<>，\!>，\!\<  
范围运算符          between… and，not between… and  
列举运算符          in，not in  
模糊匹配运算符 like，not like  
空值运算符          is null，is not null  
逻辑运算符          and，or，not

##### 1.使用比较运算符

```命令
select * from tb_name where <字符型字段> >=n;
```

##### 2.指定范围

用于指定范围的关键字有两个：between…and和 not between…and。

```命令
select * from tb_name where [not] between <表达式1> and <表达式2>;
```

其中：between关键字之后的是范围的下限（即低值）,and关键字之后的是范围的上限（即高值）  
用于查找字段值在（或不在）指定范围的行。

##### 3.使用列举

使用in关键字可以指定一个值的集合，集合中列出所有可能的值，当表达式的值与集合中的任一元素个匹配时，即返回true，否则返回false。

```命令
select * from tb_name where <字符型字段> [not] in(值1,值2,...,值n);
```

##### 4.使用通配符进行模糊查询

可用like 子句进行字符串的模糊匹配查询，like子句将返回逻辑值（true或False）。  
like子句的格式： select \* from tb\_name where \<字符型字段> \[not\] like \<匹配串>；  
其含义是：查找指定字段值与匹配串相匹配的记录。匹配串中通常含有通配符\%和\_（下划线）。  
其中:  \%：代表任意长度（包括0）的字符串

##### 5.使用null的查询

当需要判定一个表达式的值是否为空值时，使用 is null关键字。  
当不使用not时，若表达式的值为空值，则返回true，否则返回false；当使用not时，结果刚好相反。

```命令
select * from tb_name where <字符型字段> is [not] null;
```

##### 6.多重条件查询\(使用逻辑运算符\)

逻辑运算符and（与：两个条件都要满足）和or（或：满足其中一个条件即可）可用来联接多个查询条件。and的优先级高于or,但若使用括号可以改变优先级。

```命令
select * from tb_name where <字符型字段> = 'volues' and <字符型字段> > n;
```

#### \(四\)对查询结果排序

order by子句可用于对查询结果按照一个或多个字段的值（或表达式的值）进行升序（ASC）或降序（DESC）排列，默认为升序。

```命令
order by {排序表达式[ASC|DESC]}[,...n]；
```

其中：排序表达式既可以是单个的一个字段，也可以是由字段、函数、常量等组成的表达式，或一个正整数。  
模板：select \* from tb\_name order by \<排序表达式> \<排序方法>；

#### \(五\)使用统计函数\(又称集函数，聚合函数\)

在对表进行检索时，经常需要对结果进行计算或统计，T-SQL提供了一些统计函数（也称集函数或聚合函数），用来增强检索功能。统计函数用于计算表中的数据，即利用这些函数对一组数据进行计算，并返回单一的值。  
常用统计函数表  
  函数名      功能  
  AVG         求平均值  
  count        求记录个数，返回int类型整数  
  max          求最大值  
  min           求最小值  
  sum          求和

##### 1\. SUM和AVG

功能：求指定的数值型表达式的和或平均值。

```命令
select avg(<字符型字段>) as 平均数,sum(<字符型字段>) as 总数 from tb_name where <字符型字段> ='字符串';
```

##### 2\. Max和Min

功能：求指定表达式的最大值或最小值。

```命令
select max(<字符型字段>) as 最大值,min(<字符型字段>) as 最小值 from tb_name;
```

##### 3\. count

该函数有两种格式：count\(_\)和count\(\[all\]|\[distinct\] 字段名），为避免出错，查询记录个数一般使用count\(_\)，而查询某字段有几种取值用count\(distinct 字段名）。

###### \(1\).count\(\*\):

功能：统计记录总数。

```命令
select count(*) as 总数 from tb_name;
```

###### \(2\).count\(\[all\]|\[distinct\] 字段名）

功能：统计指定字段值不为空的记录个数，字段的数据类型可以是text、image、ntext、uniqueidentifier之外的任何类型。

```命令
select count(<字符型字段>) as 总数 from tb_name;
```

#### \(六\)对查询结果分组

group by子句用于将查询结果表按某一列或多列值进行分组，列值相等的为一组，每组统计出一个结果。该子句常与统计函数一起使用进行分组统计。

```命令
group by 分组字段[,...n][having <条件表达式>]；
```

##### 1.在使用group by子句后

select列表中只能包含：group by子句中所指定的分组字段及统计函数。

##### 2.having子句的用法

having子句必须与group by 子句配合使用，用于对分组后的结果进行筛选（筛选条件中常含有统计函数）。

##### 3\. 分组查询时不含统计函数的条件

通常使用where子句；含有统计函数的条件,则只能用having子句。

```命令
select <字符型字段>,count(*) as 列标题 from tb_name where <字符型字段>='字符串' group by <字符型字段>；
```

##### 4、修改数据\(Update）

Update 语句用于修改表中的数据。

```命令
update tb_name set 列名称 = 新值 where 列名称 = 某值；
```

##### 5、删除数据\(Delete\)

删除单行

```命令
delete from tb_name where 列名称 = 某值；
```

删除所有行  
可以在不删除表的情况下删除所有的行。这意味着表的结构、属性和索引都是完整的：

```命令
delete * from tb_name   或  delete from tb_name；
```

## 二、MySQL——alter命令

alter add命令用来增加表的字段。  
alter add命令格式：alter table 表名 add字段 类型 其他;

例如，在表MyClass中添加了一个字段passtest，类型为int\(4\)，默认值为0：

```例子
mysql> alter table MyClass add passtest int(4) default '0';
```

添加两个字段

```例子
mysql> alter table Person add age int,add address varchar(11);
```

删除两个字段

```例子
mysql> alter table Person drop column age,drop column address;
```

修改字段的注释

```例子
mysql> alter table `student` modify column `id` comment '学号';
```

### 1\)加索引

mysql> alter table 表名 add index 索引名 \(字段名1\[，字段名2 …\]\);

```例子
mysql> alter table employee add index emp_name (name);
```

### 2\)加主关键字的索引  
mysql> alter table 表名 add primary key \(字段名\);

```例子
mysql> alter table employee add primary key(id);
```

### 3\)加唯一限制条件的索引

mysql> alter table 表名 add unique 索引名 \(字段名\);

```例子
mysql> alter table employee add unique emp_name2(cardnumber);
```

### 4\)删除某个索引

mysql> alter table 表名 drop index 索引名;

```例子
mysql>alter table employee drop index emp_name;
```

### 5\)添加字段

```例子
mysql> ALTER TABLE table_name ADD field_name field_type;
```

### 6\)修改原字段名称及类型

```例子
mysql> ALTER TABLE table_name CHANGE old_field_name new_field_name field_type;
```

### 7\)删除字段

```例子
MySQL ALTER TABLE table_name DROP field_name;
```

## 三、MySQL – 应用

学生-课程数据库  
 学生表：Student（Sno，Sname，Ssex，Sage，Sdept）  
 课程表：Course（Cno，Cname，Cpno，Ccredit）  
 学生选课表：SC（Sno，Cno，Grade）  
 关系的主码加下划线表示。各个表中的数据示例如图所示：

### 一、建立一个“学生”表Student

```例子
create table Student(
  Sno char(9) peimary key, /*列级完整性约束条件，Sno是主码*/
  Sname char(20) unique, /* Sname取唯一值*/
  Ssex char(2),
  Sage smallint,
  Sdept char(20)
);
```

### 二、建立一个“课程”表Course

```例子
create table Course(
  Sno char(4) primary key, /*列级完整性约束条件，Cname不能取空值*/
  Sname char(40) not null, /*Cpno的含义是先修课*/
  Cpno char(4)
  Ccredit smallint,
  foreign key (Cpnoo) references Course(Cno) /*表级完整性约束条件，Cpno是外码，被参照表是Course，被参照列是Cno*/
);
```

### 三、建立学生选课表SC

```例子
create table SC(
  Sno char(9),
  Cno char(4),
  Grade smallint,
  frimary key (Sno,Cno), /*主码由两个属性构成，必须作为表级完整性进行定义*/
  foreign key (Sno) references Student(Sno), /*表级完整性约束条件，Sno是外码，被参照表是Student*/
  foreign key (Cno) references Course(Cno)   /*表级完整性约束条件，Cno是外码，被参照表是Course */
);
```

本文转载于[MySQL基础 — 常用命令](https://blog.csdn.net/qq_38328378/article/details/80858073)
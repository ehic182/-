# MySQL

## (一)基础

### 1.DDL语句：定义数据库，表，字段

​		   show databases； **查询所有数据库**

​		   select database() ；**查询当前数据库**

​		   create database 库名；  **创建数据库**

​		   drop database 【if exists】库名； **若存在删除数据库**

​		   use 库名 ；**使用数据库**

​		   show tables； **查询当前库所有表结构 要先用use使用数据库**

​		   desc  表名 ；  **查询表结构**

​		   creat table 表名(

​			字段1    字段1类型 [comment '注释'],

​			字段2    字段2类型[comment '注释'],

​			字段3    字段3类型[comment '注释'],

​			字段4    字段4类型[comment '注释']

​		);

​		show create table 表名； **查询指定表的建表语句**

​		alter table 表名 add 字段名 类型(长度)；**添加字段**

​		alter table 表名 modify 字段名 新类型(长度)；**修改字段类型**

​		alter table 表名 change 旧字段名 新字段名 类型(长度)；**修改字段名和字段类型**

​		alter table 表名 drop 字段名； **删除字段**

​		alter table 表名 rename to 新表名；**更改表名**

​		drop table if exists 表名；**删除表**

### 2.DML添加数据

​		insert into 表名 (字段名1，字段名2，......)values(值1，值2，.......);**给指定字段添加数据**

​		insert into 表名 (字段名1，字段名2，......)values(值1，值2，.......)，(值1，值2，.......)，......;**批量添加字段**

​		update 表名 set 字段名1=值1，字段名2=值2，.....【where 条件(限定特定字段)】 ; **修改数据**

​		delete from 表名 [where]；**删除数据**

### 3.DQL查询数据

select 字段名1，字段名2,字段名3......... form 表名  **查找数据**

select 字段名 as'备注名' form 表名 **查找数据，并更直观的显示**

select distinct 字段列表 from 表名 **查找数据，并去除重复选项**

select 字段名1，字段名2,字段名3......... form 表名 where 查询限定条件  **根据限定条件查找数据** 

![Screenshot_20260809_172605](D:\qq\存储/Screenshot_20260809_172605.jpg)

#### 3.1聚合函数

count 统计数量

max最大值

min 最小值

avg平均值

sum求和

以上作用于字段所在列

#### 3.2分组查询（几个不同种类分几组）

select 字段列表 from 表名 [where 条件] group by分组字段名[having 分组后过滤条件]

where是对分组之前的数据进行过滤，having是对分组之后的结果进行过滤

#### 3.4排序查询

select 字段列表 from 表名 order by字段1 排序方式1，字段2 排序方式2；//

字段一排序完之后再根据字段2进行排序

排序方式

asc 升序

desc 降序

#### 3.5分页查询

select 字段列表 from表名 limit 开始索引，查询记录数

索引=（查询页数-1）*查询记录数

#### 3.6DQL执行顺序

 

![Screenshot_20260811_151748](D:\qq\存储/Screenshot_20260811_151748.jpg)

### 4.DCL管理数据库，控制数据库的访问权限

use mysql 查询用户

select *from user;

creat user '用户名'@‘主机名’ identified by '密码' ； **创建用户**

alter user '用户名'@‘主机名’ identified witth mysql_native_password by '新密码'； **修改用户密码**

drop user '用户名'@'主机名'；

注：主机名可以用%通配

### 5.函数

### 6.约束

概念：作用于表中字段上的规则，用于限制存储在表中的数据

目的：保证数据库的完整性，正确性，有效性

**非空约束**:     not null             数据不能为空

**唯一约束** :    unique              数据唯一，不重复

**主键约束** :    primary key     主键是一行数据的唯一标识，要求非空且唯一

**默认约束**:     default              保存数据时未指定该字段的值，采用默认值 

**检查约束**：  check                 保证字段满足某一条件

**外键约束**：  foreign key        用来让两张表之间建立连接，保证数据完整性和一致性

​		    alter table 子表名 add constraing 外键名称 foreign key （外键字段名） references 父表(父表列名)  on update cascade on delete cascade 【父表和子表关联，一个改变，另一个随之改变】

​		    alter table 主表名 drop foregin key 外键表名     删除外键

外键：即关联外部表的字段

被引用主键的是父表，未引用主键的是子表

### 7.多表查询

#### 7.1内连接查询

概念：两张表的交集部分

 隐式内连接：select 字段列表 from 表1，表2 where 条件

显示内连接：select字段列表 from 表1inner join 表2 on 连接条件 更推荐这种

#### 7.2外连接查询

左外连接：select 字段列表 from 表1 left outer join 表2 on条件

  **注：查询左表的所有数据，包含表左表和右表交集部分的数据**

右外连接：select 字段列表 from 表1 right outer join 表2 on条件

 **注：查询右表的所有数据，包含表左表和右表交集部分的数据**

#### 7.3自连接查询

自连接：select 字段列表 from 表1 别名a  join 表1别名b on条件 

**注：这里必须起别名**

#### 7.4联合查询

概念：union查询即联合查询就是，把多次查询结果合并起来，形成一个新的查询结果集

select 字段列表 from 表1

union[all]

select 字段列表 from 表2

**注：多张表的列数必须保持一致，字段类型也要保持一致**

​	**union all会将所有数据直接合并在一起，union会对合并后的数据去重**

#### 7.5子查询

select *from t1 where column1=（select column from t2）；

**注：子查询外部语句可以是insert/update/delete/select的任何一个**

根据查询结果不同分为：标量查询（查询结果为单个值）

​					   列子查询（查询结果为一列）

​					    常用操作符：in(指定范围内多选一)

​								  not in(不在指定范围内)

​								   any（子查询返回列表中，有任意一个满足即可）

​								   some（与any相同）

​								    all（子查询返回列表的所有值都必须满足）

​					   行子查询（查询结果为一行）

​					   表子查询（查询结果为多行多列）

​					    常用查询：in

根据查询位置分为：where后，from后，select后

### 8.事务

查看/设置事务提交方式

select @@autocommit；自动提交事务

set @@autocommit =0; 手动提交事务

提交事务

commit；

回滚事务

rollback

四大特性：原子性：事务是不可分割的最小操作单元，要么全部成功，要么全部失败

​		   一致性：事务完成时，必须使所有数据都保持一致状态

​		   隔离性：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行

​		   持久性：事务一旦提交或回滚，他对数据库的改变就是永久的

并发事务问题：脏读：一个事务读到另一个事务还没提交的数据

​			   不可重复读：一个事务先后读取同意条记录，但两次读取的数据不同，称之为不可重复读

​		           幻读：一个事务按照条件查询数据时，没有对应的数据行，但在插入数据时，又发现这行数据已经存在，好像出现了

事务隔离级别：

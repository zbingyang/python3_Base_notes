# mysql数据库操作大全

一、概念：
-----------------------------------
```
数据： data

数据库： DB

数据库管理系统：DBMS

数据库系统：DBS

MySQL：数据库  

mysql：客户端命令（用来连接服务或发送sql指令）

SQL：结构化查询语言 ，其中MySQL支持这个。

SQL语言分为4个部分：DDL（定义）、DML（操作）、DQL（查询）、DCL（控制）

MySQL->库->表->数据

SQL语句中的快捷键

\G 格式化输出（文本式，竖立显示）

\s 查看服务器端信息

\c 结束命令输入操作

\q 退出当前sql命令行模式

\h 查看帮助

```



二、连接数据库：
-----------------------------------
```
mysql -h 主机名 -u 用户名  -p密码  库名

C:>mysql  --采用匿名账号和密码登陆本机服务

C:>mysql -h localhost -u root -proot   --采用root账号和root密码登陆本机服务

C:>mysql -u root -p   --推荐方式默认登陆本机

 Enter password: 

C:>mysql -u root -p lamp61  --直接进入lamp61数据库的方式登陆

```



三、授权：
-----------------------------------
```
格式：grant 允许操作 on 库名.表名 to 账号@来源 identified by '密码';

--实例：创建zhangsan账号，密码123，授权lamp61库下所有表的增/删/改/查数据,来源地不限

```





四、MYSQL的基本操作
-----------------------------------
```
mysql> show databases;         --查看当前用户下的所有数据库

mysql> create database [if not exists] 数据库名; --创建数据库

mysql> use test;            --选择进入test数据库

mysql> show create database 数据库名\G      --查看建数据库语句

mysql> select database();         --查看当前所在的数据库位置

mysql> drop database [if exists] 数据库名;    --删除一个数据库

mysql> show tables; --查看当前库下的所有表格

mysql> desc tb1;  --查看tb1的表结构。

mysql> show create table 表名\G  --查看表的建表语句。

mysql> create table demo( --创建demo表格

-> name varchar(16) not null,

-> age int,

-> sex enum('w','m') not null default 'm');

Query OK, 0 rows affected (0.05 sec)



mysql>drop table if exists mytab;  -- 尝试删除mytab表格

--添加一条数据

mysql> insert into demo(name,age,sex) values('zhangsan',20,'w');

Query OK, 1 row affected (0.00 sec)

mysql> insert into demo values('lisi',22,'m'); --不指定字段名来添加数据

Query OK, 1 row affected (0.00 sec)

mysql> insert into demo(name,age) values('wangwu',23); --指定部分字段名来添加数据

Query OK, 1 row affected (0.00 sec)

--批量添加数据

mysql> insert into demo(name,age,sex) values('aaa',21,'w'),("bbb",22,'m');

Query OK, 2 rows affected (0.00 sec)

Records: 2  Duplicates: 0  Warnings: 0

mysql> select * from demo; --查询数据

mysql> update demo set age=24 where name='aaa';  --修改

Query OK, 1 row affected (0.02 sec)

Rows matched: 1  Changed: 1  Warnings: 0

mysql> delete from demo where name='bbb';  --删除

Query OK, 1 row affected (0.00 sec)

mysql>\h   -- 快捷帮助

mysql>\c   -- 取消命令输入

mysql>\s   -- 查看当前数据库的状态

mysql>\q   -- 退出mysql命令行

```



五、 MySQL数据库的数据类型：
-----------------------------------
```
分为四大类：数值类型、字串类型、日期类型、NULL

5.1 数值类型

*tinyint(1字节) 0~255  -128~127

smallint(2字节)

mediumint(3字节)

*int(4字节)

bigint(8字节)

*float(4字节)   float(6,2)

*double(8字节)  

decimal(自定义)字串形数值

5.2 字串类型

普通字串

*char  定长字串     char(8)  

*varchar 可变字串 varchar(8)

二进制类型

tinyblob

blob

mediumblob

longblob

文本类型

tinytext

*text      常用于<textarea></textarea>

mediumtext

longtext

*enum枚举

set集合

5.3 时间和日期类型

date  年月日

time  时分秒

datetime 年月日时分秒

timestamp 时间戳

year 年

5.4 NULL值

NULL意味着“没有值”或“未知值”

可以测试某个值是否为NULL

不能对NULL值进行算术计算

对NULL值进行算术运算，其结果还是NULL

0或NULL都意味着假，其余值都意味着真

MySQL的运算符：

算术运算符：+ - * / %

比较运算符：= > < >= <= <> !=

数据库特有的比较：in，not in, is null,is not null,like, between and

逻辑运算符：and or not

like: 支持特殊符号%和_ ; 其中 %表示任意数量的任意字符，_表示任意一位字符

```



六、 表的字段约束：
-----------------------------------
```
unsigned 无符号(正数)

zerofill 前导零填充

auto_increment  自增

default 默认值

not null  非空

PRIMARY KEY 主键 （非null并不重复）

unique 唯一性   （可以为null但不重复）

index 常规索引

```



七: 建表语句格式：
-----------------------------------
```
 create table 表名(

   字段名 类型 [字段约束],

   字段名 类型 [字段约束],

   字段名 类型 [字段约束]

   ...

  );

mysql> create table stu(

-> id int unsigned not null auto_increment primary key,

-> name varchar(8) not null unique,

-> age tinyint unsigned,

-> sex enum('m','w') not null default 'm',

-> classid char(6)

-> );

Query OK, 0 rows affected (0.05 sec)



mysql> show create table stu\G  --查看建表的语句

********* 1. row *********

   Table: stu

Create Table: CREATE TABLE stu (

  id int(10) unsigned NOT NULL auto_increment,

  name varchar(8) NOT NULL,

  age tinyint(3) unsigned default NULL,

  sex enum('m','w') NOT NULL default 'm',

  classid char(6) default NULL,

  PRIMARY KEY  (id),

  UNIQUE KEY name (name)

) ENGINE=MyISAM DEFAULT CHARSET=utf8

1 row in set (0.00 sec)

mysql>

mysql> insert into stu(id,name,age,sex,classid) values(1,'zhangsan',20,'m','lamp

61');

Query OK, 1 row affected (0.00 sec)

mysql> insert into stu(name,age,sex,classid) values('lisi',22,'w','lamp61');

Query OK, 1 row affected (0.00 sec)

mysql> insert into stu(name,age,classid) values('wangwu',21,'lamp61');

Query OK, 1 row affected (0.00 sec)

mysql> insert into stu values(null,'qq',24,'w','lamp62');

Query OK, 1 row affected (0.00 sec)

mysql> insert into stu values(null,'aa',20,'m','lamp62'),(null,'bb',25,'m','lamp

63');

Query OK, 2 rows affected (0.00 sec)

Records: 2  Duplicates: 0  Warnings: 0

```






八、修改表结构
-----------------------------------
```
格式： alter table 表名 action（更改选项）;

更改选项：

1. 添加字段：

alter table 表名 add 字段名信息，例如：

-- 在user表的最后追加一个num字段 设置为int not null

mysql> alter table user add num int not null;

-- 在user表的email字段后添加一个age字段，设置int not null default 20；

mysql> alter table user add age int not null default 20 after email;

-- 在user表的最前面添加一个aa字段设置为int类型

mysql> alter table user add aa int first;

1. 删除字段：

alter table 表名 drop 被删除的字段名，例如：-- 删除user表的aa字段

mysql> alter table user drop aa;

1. 修改字段：

alter table 表名 change[modify] 被修改后的字段信息。其中：change可以修改字段名， modify 不修改，例如：

-- 修改user表中age字段信息（类型），（使用modify关键字的目的不修改字段名）

mysql> alter table user modify age tinyint unsigned not null default 20;

-- 修改user表的num字段改为mm字段并添加了默认值（使用change可以改字段名）

mysql> alter table user change num mm int not null default 10;

        

1. 添加和删除索引

-- 为user表中的name字段添加唯一性索引，索引名为uni_name;

mysql> alter table user add unique uni_name(name);

-- 为user表中的email字段添加普通索引，索引名为index_eamil

mysql> alter table user add index index_email(email);

-- 将user表中index_email的索引删除

mysql> alter table user drop index index_email;

1. 更改表名称：

ALTER TABLE 旧表名 RENAME AS 新表名

1. 更改AUTO_INCREMENT初始值:

ALTER TABLE 表名称 AUTO_INCREMENT=1

        

1. 更改表类型：

ALTER TABLE 表名称 ENGINE="InnoDB"

        

MySQL数据库中的表类型一般常用两种：MyISAM和InnoDB

区别：MyISAM类型的数据文件有三个frm(结构)、MYD（数据）、MYI（索引）。MyISAM类型中的表数据增 删 改速度快，不支持事务，没有InnoDB安全。

          

InnoDB类型的数据文件只有一个 .frm；InnoDB类型的表数据增 删 改速度没有MyISAM的快，但支持事务，相对安全。

        

九、数据的DML操作：

```



九、添加数据，修改数据，删除数据
-----------------------------------

```
1. 添加数据
   格式： insert into 表名[(字段列表)] values(值列表...);
   --标准添加（指定所有字段，给定所有的值）
   mysql> insert into stu(id,name,age,sex,classid) values(1,'zhangsan',20,'m','lamp138');
   Query OK, 1 row affected (0.13 sec)

mysql>

--指定部分字段添加值

mysql> insert into stu(name,classid) value('lisi','lamp138');

Query OK, 1 row affected (0.11 sec)

-- 不指定字段添加值

mysql> insert into stu value(null,'wangwu',21,'w','lamp138');

Query OK, 1 row affected (0.22 sec)

-- 批量添加值

mysql> insert into stu values

-> (null,'zhaoliu',25,'w','lamp94'),

-> (null,'uu01',26,'m','lamp94'),

-> (null,'uu02',28,'w','lamp92'),

-> (null,'qq02',24,'m','lamp92'),

-> (null,'uu03',32,'m','lamp138'),

-> (null,'qq03',23,'w','lamp94'),

-> (null,'aa',19,'m','lamp138');

Query OK, 7 rows affected (0.27 sec)

Records: 7  Duplicates: 0  Warnings: 0

        

1. 修改操作
   格式：update 表名 set 字段1=值1,字段2=值2,字段n=值n... where 条件
         

-- 将id为11的age改为35，sex改为m值

mysql> update stu set age=35,sex='m' where id=11;

Query OK, 1 row affected (0.16 sec)

Rows matched: 1  Changed: 1  Warnings: 0

-- 将id值为12和14的数据值sex改为m，classid改为lamp92

mysql> update stu set sex='m',classid='lamp92' where id=12 or id=14 --等价于下面

mysql> update stu set sex='m',classid='lamp92' where id in(12,14);

Query OK, 2 rows affected (0.09 sec)

Rows matched: 2  Changed: 2  Warnings: 0

        

1. 删除操作
   格式：delete from 表名 [where 条件]
   -- 删除stu表中id值为100的数据
   mysql> delete from stu where id=100;
   Query OK, 0 rows affected (0.00 sec)

-- 删除stu表中id值为20到30的数据

mysql> delete from stu where id>=20 and id<=30;

Query OK, 0 rows affected (0.00 sec)

-- 删除stu表中id值为20到30的数据（等级于上面写法）

mysql> delete from stu where id between 20 and 30;

Query OK, 0 rows affected (0.00 sec)

-- 删除stu表中id值大于200的数据

mysql> delete from stu where id>200;

Query OK, 0 rows affected (0.00 sec)

```

![C:\Users\Administrator\Desktop\笔记\01.](C:\Users\Administrator\Desktop\笔记\01.jpg.jpg)

```
where条件查询：

(1)查询班级为lamp138期的学生信息
 mysql> select * from stu where classid='lamp138';
    
(2)查询lamp138期的男生信息（sex为m）
mysql> select * from stu where classid='lamp138' and sex='m';
    
(3)查询id号值在10以上的学生信息
 mysql> select * from  stu where id>10;
    
(4)查询年龄在20至25岁的学生信息
mysql> select * from stu where age>=20 and age<=25;
mysql> select * from stu where age between 20 and 25;
    
(5)查询年龄不在20至25岁的学生信息
mysql> select * from stu where age not between 20 and 25;
mysql> select * from stu where age<20 or age>25;
    
(6)查询id值为1,8,4,10,14的学生信息
select * from stu where id in(1,8,4,10,14);
mysql> select * from stu where id=1 or id=8 or id=4 or id=10 or id=14;
    
(7)查询lamp138和lamp94期的女生信息
mysql> select * from stu where classid in('lamp138','lamp94') and sex='w';
mysql> select * from stu where (classid='lamp138' or classid='lamp94') and sex='w
    

十二：导入和导出
-----------------------------------
-- 将lamp138库导出
D:\>mysqldump -u root -p lamp138 >lamp138.sql
Enter password:

---- 将lamp138库中的stu表导出
D:\>mysqldump -u root -p lamp138 stu >lamp138_stu.sql
Enter password:

-- 将lamp138库导入
D:\>mysql -u root -p lamp138<lamp138.sql
Enter password:

-- 将lamp138库中stu表导入
D:\>mysql -u root -p lamp138<lamp138_stu.sql
Enter password:

D:\>

```






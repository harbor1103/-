# MySql相关语句

## 1启动与退出

```
启动 mycli -u -root -p machine

退出 exit
```

## 2.数据库操作

```
1，查看数据库 show databases；

2，创建数据库 create databases (name)；

3，删除数据库 drop databases (name);

4,   清屏 crtl+L

5， 查看字符集 show character set；

6， 查看数据库的字符集(默认utf8) show create database  (name）；

7， 修改数据库的字符集...............

8,    查看数据库支持的字符集 show charac set;

9,    查看相应字符集的校对规则 show collation；
```



## 3.使用数据库,对表进行操作（DDL）数据定义语言

查看库中的表 show tables;

```
0，使用数据库 	use 库名

1，创建表(table)   > CREATE TABLE  表名

​								    (
​                                -> id          int,
​                                -> name   VARCHAR(20),
​                                -> birth     date,
​                                -> math    int
​                                -> )

​    								 ;

//一张表的创建，就是对每一个字段进行设计

2，查看表的结构  / show create table 表名；

​				/  desc 表名

3，对表中的字段名进行修改 		alter table 表名 change 旧名 新名 数据类型;

4，在表中添加列							 alter table 表名	add     名字   数据类型;

5, 修改表中某一字段的数据类型 		alter table 表名 	modify  名字  新的数据类型;

6, 删除表中某一个字段 	             alter table 表名 drop  名字;

7，删除整张表						drop table 表名;

```

## 4.对表中数据进行写操作(DML数据控制语言)

```
0，	查看表 		select * from 表名; 

1，	添加一行数据(对指定的列)		insert into 表名(field1,field2,...)values（field1value，field2value.....）;

2,		对所有列进行数据添加（一行）(缺少一列就会报错)		insert into 表名 values（field1value, field2value，field3value，field4value.....）;

3,		对指定列进行数据的添加(多行)

4，	 对所有列都进行数据的添加(多行)	

5，	修改数据 （一行）	update 表名 set field=fieldvalue where field=fieldvalue;

6,		修改数据(所有行)		update 表名 set field=fieldvalue;

7,		复制表的结构(不复制数据)	create table 新表名 like 旧表名;

8,		复制表的结构和数据			create	table 新表名 select * from 旧表名;

9,		删除表中所有数据，但保留表的结构 	/delete from 表名；

​																			  /truncate table 表名；

10，	删除表中各别数据(where)(删一行)		delete from 表名 where field=fieldvalue;

11
```

## 	5.对表中数据进行各种各样的查询(DQL数据查询语言)

```
1 查询整张表 select * from 表名；

2查询一列或多列   select 列1，列2 from 表名 where field=fieldvalue；

3查询时去除重复数据(distinct) 		select distinct 列1，列2 from 表名 ；

4查询时用select取别名   		select	列1 as  别名  from 表名;

5查询时可进行运算(运算结果仅显示有效，实际表中数据没有进行改变)			select 列+-number from 表名;

6select 语句中出现的列不一定存在数据表中  	select curdate() ,列1，列2 from 表名;

7select 语句中的order by  (升序默认 asc/降序 desc)	select 列 from 表名 order by  列 desc；

8	where 子句 的枚举查询 (in)		select * from 表名 where field in （filedvalue1，fieldvalue2，fieldvalue3）；

9	where 子句 的模糊查询 like     

（通配符 _:下划线表示任意一个字符）	select * from 表名 where field like '__字符';

（通配符%:百分号表示的是0个或多个字符）	select * from 表名 where filed like '%字符%';

10	where 子句 的判空 (注意不是filed =NULL)	  				select * from 表名  where field is NULL；

11 分页查询 		

​	第一种：只有一个数字	limit m；（表示获取前m条数据）

​	第二种：有两个数字	    limit m，n；(表示从第m条开始获取n条)

​	第三种:	有两个数字	  limit m offset n;（表示跳过前n条，显示m条）

12添加主键约束（添加之后，该field列就不能再添加相同的已存在的filed了） 					alter table 表名 modify field 字符类型 primary key;

13主键自动增长，不需手动添加，就是再增加数据不需要再写主键那一列了    	alter table 表名 modify field 字符类型 auto_increment

14删除主键约束 ，一般情况下一张表只有一个主键

第一步，删除自动增长			alter table 表名 modify field datatype；

第二部，删除主键					alter table 表名  drop primary key；

15联合主键，就是绑了两个主键，只有两个主键上的记录都相同才是同一条记录(该记录不能被插入到表中)

联合主键是在创建时设立

create table 表名

（

id int，

name  varchar（20），

birth date，

math int，

primary key（id，name）

）；
```

​	

## 6数据完整性

1非空约束(not NULL)，

相当于在创建表的时候就规定，以后添加该表的时候，该field不可以为空，一定要有值

​	create table 表名

（

​	id int,

​	name varchar(20),

​	math int NOT NULL,

​	birth date NOT NULL,

​	primary key (id,name)

）

2唯一约束(unique)

该field添加记录时，不允许重复，允许为空

​	create table 表名

（

​	id int,

​	name varchar(20),

​	math int unique，

​	birth date NOT NULL,

​	primary key (id,name)

）

3默认值 default 在创建时设置

4外键约束 foreign key

意思就是：一个表的主键必须在另一个表里全部存在



表已经存在的情况下，用 alter

alter table 表名 add constraint fk_1(自己取一个名字) foreig key (field) references 表名(field);

表不存在的情况下，设置外键(注意：前提是那个被参考的表已经设置了主键，不然会报错)

create table 表名

(

id int auto_increment primary key,

name varchar(20),

c_id int，

foregin key(c_id)references 表名(field)

)

5删除外键		

​	第一步： show create table 表名;找到外键名

​	第二步：	删除	alter table 表名 drop foreign key 外键名;

## 7数据备份与恢复

1. 在终端 	$ mysqldump -u root -p 库名 > 库名.sql

   ​	然后ls ，就会看到库名.sql的文件，vim 查看一下就好了

2. 恢复数据库

   ​	先要在mysql服务器上创建一个空的数据库

   第一种：在外面，在命令行下输入 		mysql -u root -p 新库名<库名.sql;

   第二种：在mysql里面	use 新库名;

   ​										 source 库名.sql;			

## 8复杂查询(多表查询)

 1交叉连接(笛卡尔积)	A表有m行，B表有n行，合并之后的临时表是mxn行

三种写法

第一种 select * from 左表 cross join 右表;

第二种 select * from 左表 join 右表;

第三种 select * from 左表，右表;（隐式写法）

2内连接 inner join

当内连接不加on的时候与交叉连接的效果相同

使用inner join 关键字 ，在on子句中设定连接条件

select * from 左表 inner join 右表 on 左表.field=右表,field;

3 外连接 

左外连接与右外连接

左外连接：	当右表中没有与之对应的记录时，查询出的右表中的数据全部用NULL代替，左表中的所有数据都出现

(意思就是:当内连接时，数据相当才会出现，而在外连接中，数据相等左右表都出现，其他情况，左表出现，右表为NULL)

select * from 左表 left join 右表 on 左表.field=右表.field；

右外连接:		同理



4子查询

嵌套查询

举例子

select * from student where id in(select id from student where id>3);



5联合查询，就是合并两个查询结果，然后去除重复的记录，然后没有重复的查询记录(union)

举例子：	

select * from student where math >80 union select * from student where chinese >70;

6分组查询 group by

用group by 的时候会对前面的select选择有限制，

select field from 表名 where ...  group by field;

分组之后，要对数据进行处理：

（1）count：统计个数

​		select count (field) from 表名 where ... group by field;

(2)max

(3)min

举例：

SELECT count(math),max(id),min(id),math from student where id >2 GROUP BY math;

这句话的意思是，我要统计在student这个表中，在id大于2的数据中找出math这个组，然后我要count math这个组中，不同的math有几个，然后分别在不同的math找出max id与min id

（4）分组之后，若还要对记录进行过滤，只能使用having子句，不能使用where子句；

举例：

SELECT COUNT(math),math,max(id),min(id)FROM student WHERE id>2  GROUP BY math HAVING math>100;



## 9关键字使用的一些顺序

select count(field) from 左表  join_type 右表 on........	where .....

group by	........ having...... order by.........



## 10还有一些其他函数

1.sum

SELECT sum(id) from student;





## 11C语言的API






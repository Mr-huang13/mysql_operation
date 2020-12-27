# 数据库实施

## 1. 创建与管理

查询当有那些库：`show databases;`

创建数据库：` alter database db_name;`

删除数据库：` drop database db_name;` 

## 2. 表的操作

查询当前库有那些表：`show table`

创建表：` create table`

***完整格式：*** `create table tbl_name(`属性名 数据类型 [完整性约束条件],属性名 数据类型 [完整性约束条件],primary key(属性名));` 

查看表结构：`describe tbl_name;`

增加字段：`alter table 表名 add 属性名 数据类型 [完整性约束条件]`

修改字段类型：` alter table 表名 modify 字段名 数据类型`

删除字段：`alter table 表名 drop column 字段名 `

删除表：`drop table db_name;`





## 3. 数据的操作

插入数据：`insert into tbl_name (field1,field2...) values(value1,values2...);`

修改数据：`update tbl_name set field1=newfield1,field2=newfield2 [where Clause]`

删除数据：`delete from tbl_name [where Clause]`





## 3. 数据检索

官网`select`语法：

`SELECT column_name1, column_name2 `

`FROM table_name `

`[WHERE where_condition] `

`[GROUP BY {col_name | expr | position}, ... [WITH ROLLUP]] `

`[HAVING where_condition]`

 `[ORDER BY {col_name | expr | position} [ASC | DESC], ... [WITH ROLLUP]] ` 

`[LIMIT {[offset,] row_count | row_count OFFSET offset}]`



进入库：`use db_name;`

基本查询：`select * from tbl_name;`

#### 单表查询 ：

**筛选：**

- where
- having

**范围查询：**

- < 、>

- in 
- between

**模糊查询：**

- like

**查询空值：**

`where column is [not] null`

**多条件查询：**

- or  
- and

**排序（order by）、分页（limit）:**

- ` order by [DESC]`
- ` limit 2,3`(显示3到5行数据)

**聚合函数：**

- count()
- sum()
- avg()
- max()、min()

#### 多表连接 ：

**内连接**

例：查询选修课程号为c05109的学生的学号、姓名、和期末成绩

```mysql
select student.sno,sname,final 
	from student,score
	where student.sno=score.sno
	and score.cno='c05109';
```



# 数据库维护

## 1. 索引

```mysql
##普通索引
use db01;
create index name_index on abl_name(finde1);
##唯一索引
create unique index name_index on abl_name(finde1);
##复合索引
create index name_index on abl_name(finde1,finde2);
##删除索引
drop index index_name on tbl_name;
```

## 2. 视图

**创建**

```mysql
create view name_view
as select finde1,finde2...
from tbl_name
[where Clause]
```

**删除**

`drop view name_view;`

## 3. 存储过程

**创建**

例：创建一个存储过程，

​		查询出期末成绩低于60分的学生，显示不及格学生名单，显示字段包括学		号，课程号，平时成绩，期末成绩。

​		如果没有低于60分的学生，显示“大吉大利，今晚吃鸡”。

```mysql
delimiter //
create procedure proc_name()
begin
declare str char(20) default '大吉大利，今晚吃鸡';
declare n int default 0;
select count(*) into n 
from score 
where final<60;
if n>0 then
select sno,cno,final1,final2 
from score 
where final<60;
else n<=0 then select str;
end if;
end //
delimiter ;
```

## 4. 触发器

**创建**

例：创建一个触发器，当删除student表某个人的记录时，删除sc表相应的成绩记录。

```mysql
delimiter //
create trigger stu_selete after delete
on student for each row
begin
delete from sc where sno=old.sno;
end //
delimiter;
```

例：创建一个触发器，当更改表course中某门课的课程编号时，同时将sc表的课程全部更新。

```mysql
delimiter //
create trigger con_update after update
on course for each row
begin
update score set cno=new.con
where con=old.con;
end//
delimiter;
```

**删除**

`drop trigger name_trigger;`

## 5. 备份与删除

**备份**

`mysqldump -u root -p db01 student sc course course>D:\daima\1\news2.sql`

**还原**

`mysql -u root -p db02<D:\daima\1\news2.sql`
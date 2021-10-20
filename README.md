# 常用sql语句

## 查看数据库
```sql
show database;
```

## 创建数据库
```sql
create database database_name;
```

## 切换数据库
```sql
use database_name;
```

## 查看某数据库中所有的数据表
```sql
show table;
```

## 创建数据表
```sql
CREATE TABLE my_table (
    name VARCHAR(20),
    owner VARCHAR(20),
    species VARCHAR(20),
    sex CHAR(1), 
    birth DATE, 
    death DATE
);
```

## 查看数据表结构
```sql
describe  table_name;  --缩写: desc
```

## 查看数据表中的记录
```sql
select  *  from  table_name;

-- 去重复
select distinct name from table_name
```

## 往数据表中添加数据记录
```sql
INSERT INTO table_name
VALUES('puffball','Diane','hanst','f','1999-03-23',NULL);

-- 指定属性
insert into user3 (name) value('asfjl');
```

## 删除数据
```sql
delete from table_name where name='puffball';
```

## 修改数据
```sql
update table_name set name='wang' where owner='haha'
```

## 建表约束--主键
```sql
create table user(
    id int primary key,
    name varchar(20)
); 

 -- 联合主键
create table user2(
    id int,
    name varchar(20),
    password varchar(20),
    primary key(id,name) 
);

-- 后来添加主键
create table user4(
    id int,
    name varchar(20)
);
alter table user4 add primary key(id);

-- 删除主键约束
alter table user4 drop primary key;

-- 修改约束
alter table user4 modify id int primary key;
```

## 建表约束--自增
```sql
create table user3(
    id int primary key auto_increment,
    name varchar(20)
);
```

## 建表约束--唯一：约束修饰的字段的值不可以重复
```sql
create table user5(
    id int,
    name varchar(20),
    unique(name)     
-- 可一起约束多个，不一起重复即可；unique(id,name) 
);

-- 再另一种写法
create table user5(
    id int,
    name varchar(20)
);
alter table user5 add unique(name);

-- 删除唯一约束
alter table user5 drop index name

-- modify
alter table user5 modify name varchar(20) unique;
```

## 非空约束：修饰的字段不能为NULL
```sql
create table user6(
    id int,
    name varchar(20) not null 
);
-－ 反null? 异常
insert into user6 (name) value('jfsl');
```

## 默认约束
```sql
create table user7(
    id int,
    name varchar(20),
    age int default 10
);

 insert into user7 (id,name) value(1,'slfj');
 insert into user7 (id,name,age) values(1,'slsfj',5);
```

## 外键约束
```sql
create table classes(
    id int primary key,
    name varchar(20)
);
create table students(
    id int primary key,
    class_id int,
    foreign key(class_id) references classes(id)
 );
 ```
 
 ## 多表查询
 ```sql
 -- 两表查询
select sname,cno, degree from student,score
where student.sno = score.sno;

-- 三表查询
select sname, cname,degree from  student,course,course,score
where student.sno = score.sno
and course.cno = score.cno;
```

## 分组查询
```sql
-- 子查询加分组求评均
select cno, avg(degree) from score
where sno in (select sno from student where class='1233')
group by cno;

-- year函数与带in关键字的子查询
select * from student where year(sbirthday) in (select year(sbirthday) from student where sno in (108,117));
```

## 多层嵌套查询
```sql
select tno from teacher where tname='zhang';
select cno from course where tno = (select tno from teacher where tname='zhang');

select * from score where cno=()
select * from score where cno=(select cno from course where tno =(select tno from reacher where tname='zhang'));
```

## union与not in
```sql
-- not in
select prof from teacher where depart='电子工程系';
select * from teacher where depart='计算机系' and prof not in (select 
prof from teacher where depart='电子工程系');

select * from teacher where depart='电子工程系' and prof not in (select prof from teacher where depart='计算机系');

-- UNION 操作符用于合并两个或多个 SELECT 语句的结果集
select * from teacher where depart='计算机系' and prof not in (select prof from teacher where depart='电子工程系')
union
select * from teacher where depart='电子工程系' and prof not in (select prof from teacher where depart='计算机系');
```

## any与all
```sql
-- any表示至少一个
select * from score where cno='34'
and degree>any(select degree from score where cno='334')
order by degree desc;

-- all表示所有
select * from score where cno='34'
and degree>all(select degree from score where cno='334')
order by degree desc;
```

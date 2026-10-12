# DDL 
## 数据库操作
1. show databases;
2. create database [if exists]数据库名[default charset];
3. drop database[if exists]数据库名;
4. use database 数据库名;  
5. select database() ;  //查询当前数据库是哪个数据库
## 表操作
1. create table 表名 （
	字段名  字段类型[comment] 
     ) [comment];   //建表
2. show tables;  
3. desc 表名;    //查询表结构
4. show create table表名;    //查询建表语句
5. alter table 表名 add 字段名 字段类型  //添加字段
6. alter table 表名 modify 字段名  字段类型  //修改字段类型
7. alter table 表名 change 字段名 新字段名 字段类型 //修改字段名称和字段类型
8. alter table 表名 drop 字段名 字段类型  //删除字段
9. drop table 表名 //删除表结构
# DML
1. insert into 表名 (字段名1，字段名2...) values(数据1，数据2...) //给字段插入数据，数据个数要等于字段个数
2. insert into 表名  values(数据1，数据2...)  //省略了字段名
3. update 表名 set 字段1=值1，字段2=值2... [where 条件] // 修改字段数据，不加where则修改全部字段数据
4. delete from 表名 [where 条件] //删除满足条件的一整行，不加where则删除整个表的所有数据
# DQL
## 基础查询
1. select 字段1[(as)别名1]，字段2[(as)别名1]...from 表名  //as可以省略
2. select * from 表名 //查询所有字段数据
3. selcet distinct 字段列表 from 表名 //去除重复记录
## 条件查询 
1. select 字段列表 from 表名 where 条件列表 







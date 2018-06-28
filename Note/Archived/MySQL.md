># 安装
sudo apt-get install mysql-server mysql-client
***
># 基本操作
	create database 数据库名;
	use 数据库名;
	create table 表名(字段名 类型 [not ]null[ primary key], ...);
	insert into 表名 values (条目);
	select * from 表名
	alter table 表名 add 字段名 类型 [not ]null; 添加新字段
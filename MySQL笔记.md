# MySQL基础学习笔记

[看B站遇见狂神说的视频](https://www.bilibili.com/video/BV1NJ411J79W)

 ## 连接数据库

```sql
mysql -u root -p密码 -- 连接数据库

-- 修改用户密码
update mysql.user 
set authentication_string = password('密码')
where user='root' and Host='localhost';

-- 刷新权限
flush privileges;

-- 显示所有数据库
show databases;

-- 切换数据库
use 数据库

-- 显示数据库中所有的表
show tables;

-- 显示表结构
describe 表名;
desc 表名;

-- 退出链接
exit;

-- 创建数据库
create database 数据库名;
```

DDL 定义，DML 操作，DQL 查询，DCL 控制

## 操作数据库

```sql
-- 创建数据库
create database [if not exists] 数据库名; -- 如果数据库不存在就创建

-- 删除数据库
drop database 数据库名;

drop database if exists 数据库名; -- 如果存在 就删除
```

如果数据库名字是一个特殊的字符，需要使用 

```sql
drop database `据库名`;
```

## 数据类型

**数值：**

- `tinyint` 十分小的数据 1 个字节
- `smallint` 较小的数据 2 个字节
- `mediumint `  3个字节
- `int`    4个字节
- `bigint`   8个字节
- `float` 浮点数 4 个字节
- `double` 浮点数 8个字节
- `decimal` 字符串形式的浮点数 ， 金融计算

**字符串：**

- char 字符串固定大小的   0~255
- varchar 可变字符串 0~65535
- tinytext 微型文本  2^8 -1
- text 文本串 2^16 -1

**时间日期：**

- date   YYYY-MM-DD ，日期
- time HH:mm:ss  时间格式
- datetime   YYYY-MM-DD  HH:mm:ss
- timestamp  时间戳  1970.1.1 到现在的毫秒数

**NULL值：**

- 没有值，未知



## 数据库的字段属性

- unsigned ：无符号整数，不能为负数
- zerofill： 0 填充，不足的位数 使用0在前面填充
- 自增：在原来的基础上 加一
- Not NULL：不赋值 就报错
- 默认：不赋值，就使用 默认值

## 创建表

```sql
-- 创建 student 表
create table if not exists `student`(
	`id` int(4) not null auto_increment  comment '学号',
	`name` varchar(30) not null default '匿名' comment '姓名',
	`pwd` varchar(20) not null default '123456' comment '密码',
	`sex` varchar(2) not null default '女' comment '性别',
	`birthday` datetime default null comment '生日',
	`address` varchar(100) default null comment '家庭住址',
	`email` varchar(50) default null comment '邮箱',
	primary key(`id`)
)engine=innodb default charset=utf8

-- auto_increment 设置自增
-- default 设置默认值
-- comment 注解说明
-- not null 设置非空
-- primary key 设置主键
-- engine 指定引擎
-- charset 指定字符集编码
```

```sql
-- 查看创建数据库的语句
show create database 数据库名;

-- 查看表 的创建语句
show create table 表名;
```



## 引擎

|            | MyISAM | Innodb              |
| ---------- | ------ | ------------------- |
| 事务的支持 | N      | Y                   |
| 行锁       | N      | Y                   |
| 外键       | N      | Y                   |
| 全文索引   | Y      | N(新版本好像支持了) |
| 表空间大小 | 较小   | 较大，约为两倍      |

- MyISAM：占用空间小，速度快
- InnoDB：安全性高，事务处理，多表多用户。

MyISAM 里对应的文件

- *.frm 表结构的定义文件
- *.MYD 数据文件
- *.MYI 索引文件

## 删除表

```sql
-- 修改表名
alter table oldTableName rename as newTableName;

-- 在表中添加字段
alter table table_name add 字段名 列属性

-- 修改表的约束
alter table table_name modify 字段名 新的属性

-- 修改字段名
alter table table_name change 旧字段名 新字段名;

-- 删除字段
alter table table_name drop 字段名

-- 删除表
drop table if exists 表名; -- 如果 表存在就删除
```

## 外键

```sql
-- 定义 外键在 建表时
create table if not exists `student`(
	`id` int(4) not null auto_increment  comment '学号',
	`gradeid` int(10) not null comment '学生年级',
    ....
    -- 定义主键
	primary key(`id`),
    -- 声明一个键
    key `FK_gradeid` (`gradeid`),
    -- 设置外键
	constraint `FK_gradeid` foreign key (`gradeid`) references `grade`(`gradeid`)
)engine=innodb default charset=utf8

-- key 键名 (本表中的属性，就是外键)
-- constraint 定义的键名 foreign key (本表中外键) references 引用的表(引用表中对应的外键列)

-- 创建表的时候没有外键关系
-- 添加外键

alter table `student` add constraint `FK_gradeid` foreign key(`gradeid`) references `grade`(`gradeid`)

-- 添加外键
alter table 表名 add constraint 键名 foreign key(自己表中字段) references 引用表(引用表中的字段);
```

- 最佳实践：
  - 数据库就是单纯的表，只用来存储数据，只有行和列
  - 想使用多张表的数据，使用程序实现，不是外键

## DML

```sql
-- 添加数据元素
insert into 表名 (字段名1,字段名2,...,字段n) values (值1, 值2,...,值n);
-- 不指定 字段名 ，默认就是表中字段的顺序 , 后面的值和表中字段顺序一一对应

-- 添加多条数据
insert into 表名 (字段名1,字段名2,...,字段n) 
values (值1, 值2,...,值n), (值1, 值2,...,值n);

-- 更新数据
update 表名 set 字段名1 = 值1,字段名2 = 值2,... where 条件;

-- between 数 and 数 ，是闭区间

-- current_time 获取当前时间

-- 删除数据
delete from 表名 where 条件;

-- 清空表数据，保留表结构
truncate 表名;
truncate table 表名;




```

- delete 和 truncate 区别：
  - truncate 会重新设置，自增列，计数器会归零。
  - truncate 不会影响事务。
  - delete 删除数据后，重启，对于 InnoDB来说 自增会从1开始，而 MyISAM不会。
    - 因为 innodb 是存在内存中，而 MyISAM是存在 文件中。
    - 8.0 修复了

## 查询DML

```sql
-- 查询全部信息
select * from 表名;

-- 查询指定字段
select 字段名1,...,字段n from 表名;

-- 设置 别名
select 字段名 as 别名 from 表名 as 表别名;

-- 拼接字符串 使用函数 concat(a,b)
select concat('信息',字段名) from 表名;

select concat(字段名,字段名) from 表名;

-- 去除重复数据，使用 distinct 关键字
select distinct 字段名 from 表名;

-- 查看系统版本号
select version(); -- 函数

-- 可以进行表达式计算
select 100*2 as result; -- 表达式

-- 查询自增的步长
select @@auto_increment_increment; -- 变量
```

模糊查询

- is null 
- is not null
- between and
- like 
  - %  代表 零 到 任意个 字符
  - _ 代表一个字符
- in

```sql
-- 以指定 字符结尾
select 字段 from  表名 where 字段 like '%字符';

-- 以指定字符 开头
select 字段 from  表名 where 字段 like '字符%';

-- 中间
select 字段 from  表名 where 字段 like '%字符%';

-- in 的使用
select 字段 from  表名 where 字段 in (值1,值2,...值n);

```

联表查询

![](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=99855224,4005714616&fm=26&gp=0.jpg)

分页和排序

```sql
-- 排序 order by
-- asc 升序 ， desc 降序
select 字段 from 表名 order by 排序字段 asc; -- 升序
select 字段 from 表名 order by 排序字段 desc; -- 降序


-- 分页 使用 limit
-- 为了 缓解数据库 压力
select 字段 from 表名 limit 起始位置,查询的条数(页面大小);
```

子查询 where 里面再有有一个查询

## 函数

```sql
-- 常用 函数

abs() -- 绝对值
ceiling() --向上取整
floor() -- 向下取整
rand() -- 随机数0~1
sign() -- 返回参数符号，负数返回 -1

-- 字符串函数
char_length(字符串) -- 返回字符串长度
lower() -- 转小写
upper() -- 转大写
replace() -- 替换

-- 时间日期函数
 current_date() -- 获得当前日期
 now()  -- 当前时间 
 localtime() --本地时间
 sysdate() -- 系统时间
 
-- 系统
system_user() -- 系统用户
user() -- 当前用户
version() -- 版本
```

## 聚合函数

```sql
-- 
count() -- 数量
sum() -- 和
avg() -- 平均分
max() -- 最高
min() -- 最小
```

count(列) 和 count(*) 和 count(1) 的区别？

- count(列)  会忽略 null 值。count(*) 和 count(1) 不会忽略。
- count(*) 和 count(1) 的区别



## 分组 和 过滤

group by 进行分组

having 对分组进行过滤。

## 加密

使用 MD5 函数

```sql
MD5() -- 进行加密
```

## 事务

要么成功，要么失败。

```sql
-- mysql 默认是 开启事务的
set autocommit = 0 -- 关闭事务
set autocommit = 1 -- 开启事务

-- 手动处理事务


start transaction -- 标记一个事务的开始，从此处开始的SQL都在一个事务内

-- 此处执行 一些 SQL 语句 

-- 提交，进行持久化
commit 
-- 回滚
rollback -- 回到原来的样子

-- 设置保存点
savepoint 保存点 -- 设置保存点

rollback to savepoint 保存点 -- 回滚到保存到

release savepoint 保存点 -- 撤销保存点
```

![](D:\harlearn.github.io\手动事务的过程.png)

## 索引

是帮助MySQL高效获取数据的数据结构

- 主键索引 primary key
  - 唯一标识，不可重复。
- 唯一索引 unique key
  - 避免数据重复
- 常规索引 key / index
  - 默认 index
- 全文索引 full text

```sql
-- 显示 所有索引信息
show index from 表名;

-- 增加一个 全文索引
alter table 表名 add fulltext index 索引名(指定的列);

-- explain 分析 SQL 执行的状况
explain select * from 表名;

-- 给表添加索引
create index 索引名 on 表(字段);

```

## 索引原则

- 索引不是 越多越好
- 不要对经常变动的数据添加索引
- 小数据的表不需要添加索引
- 索引一般添加在经常用来查询的字段上

## 权限管理

用户管理，用户表 mysql 下的 user

```sql
-- 创建 用户
create user 用户名 indentifed by 密码;

-- 修改密码 当前用户密码
set password = password(密码);

-- 修改指定用户的密码
set password for 用户名 = password(密码);

-- 用户重命名
rename user 旧用户名 to 新用户名

-- 用户 授予全部的权限,除了 给别人授权
grant all privileges on *.* to 用户;

-- 查询权限
show grants for 用户名;

-- 撤销权限 撤销全部权限
revoke all privileges on *.* from 用户名;

-- 删除用户
drop user 用户名;
```



## 数据库备份

- 直接拷贝 物理文件

- 在 可视化 工具中手动导出

- 使用命令号 导出 ： mysqlDump 命令

  ```sql
  -- 导出
  mysqldump -h 主机 -u 用户名 -p 密码  数据库 表名 > 物理磁盘位置/文件名
  
  -- 导入 登录的情况下
  source SQL文件的位置;
  ```

## 三大范式

1. 数据表中的每一列都是不可在分割的原子数据项。（列不可再分）
2. 每张表 只做一件事
3. 每一列和主键直接相关，不能间接相关。




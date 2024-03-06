
# 了解

1. 数据库
>		数据存储的仓库

2. 数据库管理系统
>		操作

3. SQL
>		操作关系型数据库的编程语言，是一套标准

# 下载与安装

[MySQL官网](https://www.mysql.com/cn/)
***[数据库卸载](https://blog.csdn.net/weixin_56952690/article/details/129678685)***
*[MySQL绿色安装与卸载](https://developer.aliyun.com/article/910876)*

## Mysql8绿色版的安装与卸载

### 安装

1. 在官网下载对应版本的Mysql压缩包
2. 在主目录下创建`data`文件夹和`my.ini`
3. 修改`my.ini`配置文件
4. DOS窗口进行初始化
	- 第一个：*`mysqld --initialize-insecure --user=root`*  这个不好用
	- 第二个：*`mysqld --initialize --console`* mysql8这个可行
		- 会生成一个密码
5. `mysqld -install [服务名]` 服务名自己启，安装服务
6. `net start mysql8` 启动服务
7. 修改密码
	- `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码'`  
	- `mysqladmin -u root password  11111`
8. `mysql -u root -p`  进入mysql

- 遇到的问题：
	1. 配置文件的问题，网上的配置文件有一些地方存错误，按照日志可以修改，然后使用第一种的初始化方式虽然可以在控制面板上生成密码，但是无法创建root用户，在后期没有办法修改密码，所以当我们调整好配置文件后，使用第二种方式就可以了


```INI
[mysqld]
# 设置服务端口为3306
port=3306
# 设置mysql的安装目录，注意目录需要使用\\连接
basedir=D:\\CodeTools\\MySQL\\mysql-8.0.36-winx64\\mysql-8.0.36-winx64
# 设置mysql数据库的数据的存放目录，注意目录需要使用\\连接
datadir=D:\\CodeTools\\MySQL\\mysql-8.0.36-winx64\\mysql-8.0.36-winx64\\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“caching_sha2_password”插件认证
authentication_policy=caching_sha2_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```
### 卸载
1. 关闭服务
2. cmd数据regedit进入注册表删除以下目录中注册表`\HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\EventLog\Application\MySQLD Service`
3. `mysqld -remove 服务器名`卸载数据库
4. 卸载目录下的对应文件



*下载：下载安装包下面的方式，下载压缩包下载serve里面的*
### 安装包方式
第一步：![[Pasted image 20240304205034.png]]
第二步：![[Pasted image 20240304205115.png]]
第三步：![[Pasted image 20240304205155.png]]
第四步：![[Pasted image 20240304205327.png]]

*安装与启动*：


- `cmd输入services.msc`进入系统服务，进行启动和关闭

- 命令行中输入，`net start mysql80`启动；`net stop mysql80`停止

*mysql的连接：*

- 使用客户端，只需要输入密码就可以

- 使用cmd连接，记得配置环境变量，输入`mysql [-h 127.0.0.1] [-p 3306] -u root -p`输入密码即可

# SQL

## *通用语法：*

![[Pasted image 20240304212533.png]]

## *SQL分类：*

![[Pasted image 20240304212711.png]]

### *DDL:*

1. DDL-数据库操作
	- 查询
		- 查询所有数据库，`SHOW DATABASES;`
		- ，`SELECT DATABASE`
	- 创建
		- `CREATE DATABASE [ IF NOT EXISTS] 数据库名[ULT CHA DEFARSET  字符集] [ COLLATE 排序规则];`
		- 字符集可以使用`utf8mb4`
	- 删除
		- `DROP DATABASE [IF EXISTS] 数据库名;`
	- 使用
		- `USE 数据库名`
2. DDL-==表操作==-查询
	- 查询当前数据所有表，`SHOW TABLES;`
	- 查询表结构，`DESC 表名`
	- 查询指定表的建表语句，`SHOW CREATE TABLE 表名`
3. DDL-==表操作==-创建
```SQL
CREATE TABLE 表名(
字段 字段类型 [COMMENT 字段注释],
...
)[COMMENT 表注释];

-- [] 表示可选
-- 最后一个字段不需要加上,
```
4. DDL-数据类型

数值类型：![[Pasted image 20240304215554.png]]

字符串类型：![[Pasted image 20240304220249.png]]

日期类型：![[Pasted image 20240304220723.png]]

```SQL
CREATE TABLE TEST(
-- UNSIGNED的意思无符号就是全是正数
age TINYINT UNSIGNED,
-- 长度包含了小数位数
score double(长度,小数位数),
-- BLOB二进制用的不多
name char(11) , -- 定长，性能高
name varchar(11), --变长，性能低
)


CREATE TABLE EMP (
	ID INT ,
	WORKNO VARCHAR(10),
	WORKNAME VARCHAR(10),
	GENDER CHAR(1),
	AGE TINYINT UNSIGNED COMMENT '年龄',
	IDCODE CHAR(18),
	ENTRYTIME DATE
)
```

5. DDL-==表操作==-修改
	- 添加字段
		- `ALTER TABLE 表名 ADD 字段 字段类型 [COMMENT "注释"] [约束];`
	- 修改字段数据类型
		- `ALTER TABLE 表名 MODIFY 字段名 新字段类型`
	- 修改字段名和字段类型
		- `ALTER TABLE 表名 CHANGE 旧字段名 新字段名 新类型 [COMMENT "注释"] [约束]`
	- 删除字段
		- `ALTER TABLE 表名 DROP 字段`
	- 修改表名
		- `ALTER TABLE 表名 RENAME TO 新表名`
6. DDL-表操作-删除
	- 删除表
		- `DROP TABLE [IF EXISTS] 表名`
	- 删除表，然后重新创建一个新表
		- `TRUNCATE TABLE 表名;`

### DML

1. DML-添加数据
	- 给指定字段添加数据
		- `INSERT INTO 表名(字段1,字段2,...) VALUES(值1,值2,...);`
	- 给全部字段添加数据
		- `INSERT INTO 表名 VALUES(值1,值2,...);`
	- 批量添加数据
		- `INSERT INTO 表名(字段1,字段2,...) VALUES(值1,值2,...),(值1,值2,...);`
		- `INSERT INTO 表名 VALUES(值1,值2,...),(值1,值2,...);`
	- 注意：
		- 插入数据时，指定的字段顺序需要与值的顺序是一一对应的
		- 字符串和日期型数据应该包含在引号中。
		- 插入的数据大小，应该在字段的规定范围内
2. DML-修改数据
	- `UPDATE 表名 SET 字段名1 = 值1,字段名2 = 值2,... WHERE 条件`
		- 修改语句的条件可以有，也可以没有，如果没有条件，则会修改整张表的所有数据
3. DML-删除数据
	- `DELETE FROM 表名 [WHERE 条件]`
		- DELETE语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据
		- DELETE语句不能删除某一个字段的值（可以使用UPDATE）

### DQL

```sql
SELECT 
		字段列表
FROM
		表名列表
WHERE
		条件列表
GROUP BY 
		分组字段列表
HAVING
		分组后条件列表
ORDER BY
		排序字段列表
LIMIT
		分页参数
```

1. DQL-基本查询
	- 查询多个字段
		- `SELECT 字段1，字段2，字段3，...  FROM 表名;`
		- `SELECT * FROM 表名;`
	- 设置别名
		- `SELECT 字段 [as '别名'] FROM 表名;`
		- `SELECT 字段 ‘别名’ FROM 表名`
	- 去除重复记录
		- `SELECT DISTINCT 字段列表 FROM 表名`

2. 条件查询
![[Pasted image 20240305201625.png]]
![[Pasted image 20240305201652.png]]
语法：`SELECT 字段列表 FROM 表名 WHERE 条件;`

3. DQL-聚合函数(配合分组查询)、

创建聚合函数![[Pasted image 20240305202454.png]]
聚合函数写在==字段列表的位置==

4. DQL-分组查询

语法:`SEECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件]`

注意：
- where与having区别
	- 执行时机不同：where是分组之前进行过滤，不满足where条件，不参与分组；而having是分组之后对结果过滤
	- 判断条件不同：where不能对聚合函数进行判断，而having可以
- 执行顺序：where > 聚合函数 > having
- 分组之后，查询的字段一般为==聚合函数和分组字段==，查询其他字段无任何意义

5. DQL-排序查询

语法：`SELECT 字段列表 FROM 表名 ORDER BY 字段1 [排列方式]，字段2 [排列方式]，....;`
- 升序：`ASC`-默认
- 降序：`DESC`

5. DQL-分页查询

语法：`SELECT 字段列表 FROM 表名 LIMT 起始索引,查询记录数`

注意：
- 起始索引从0开始，`起始索引 = (查询页数-1)*每页显示记录数`
- 分页查询时数据库的方言，不同的数据库有不同的实现，MySQL中是LIMIT
- 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10


5. DQL的执行顺讯

> 执行顺序与编写顺序不一样

```SQL
SELECT 
		字段列表 4
FROM
		表名列表  1
WHERE
		条件列表 2
GROUP BY 
		分组字段列表 3
HAVING
		分组后条件列表
ORDER BY
		排序字段列表 5
LIMIT
		分页参数 6

```

DCL

> 管理数据库用户，控制数据库的访问权限

*用户管理*

1. 查询用户 mysql
```SQL
USE mysql;
SELECT * FROM user;
```

2. 创建用户
```sql
CREATE USER ‘用户名’@‘localhost’ IDENTIFIED BY ‘密码’

CREATE USER ‘用户名’@'%' IDENTIFIED BY ‘密码’
```

3. 修改用户密码
```SQL
ALTER USER ‘用户名’@‘主机号’ IDENTIFIED WITH mysql_native_password(加密方式) BY '新密码'
```

4. 删除用户
```SQL
DROP USER ‘用户名’@'主机名';
```

- 注意：
	- 主机名可以使用%通配
	- 开发人员用的少

*权限控制*

![[Pasted image 20240306182603.png]]

1. 查询权限
```SQL
SHOW GRANTS FOR '用户名'@‘主机名’;
```

2. 授予权限
```SQL
GRANT 权限列表 ON 数据库名.表名 TO  ’用户名‘@’主机名‘;
```

3. 撤销权限
```SQL
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

- 注意：
	- 多个权限之间，使用逗号隔开
	- 授权时，数据库名和表名可以使用*进行通配，代表所有

## 函数

> 函数式指一段可以直接被另一段程序调用的程序代码（内置的）
### 字符串函数

![[Pasted image 20240306185304.png]]

### 数值函数

![[Pasted image 20240306191715.png]]

### 日期函数

![[Pasted image 20240306192305.png]]

### 流程函数

![[Pasted image 20240306193052.png]]

![[Pasted image 20240306193552.png]]
![[Pasted image 20240306193915.png]]

## 约束

### 常用约束

![[Pasted image 20240306194112.png]]

注意：约束是作用在表中字段上的，可以在创建表/修改表的时候添加约束

### 外键约束

> 外键用来让两张表的数据之间建立连接，从而保证数据一致性和完成性

![[Pasted image 20240306195337.png]]

- 添加外键
```SQL
CREATE TABLE 表名(
	字段名 数据类型
	...
	[CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
)

ALTER TABLE ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名); 
```

注意：删除先删除从表再删除主表

- 删除外键
```SQL
ALTER TABLE 表名 DROP FOREIGN KEY 外键名程;
```

### 外键删除更新行为

![[Pasted image 20240306200851.png]]

```SQL
ALTER TABLE ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名) ON UPDATE 行为 ON DELETE 行为;
```

## 多表查询

### 多表关系介绍

- *一对多(多对一)*
	- 实现：在多的一方建立外键，指向一的一方的主键
- *多对多*
	- 实现：建立第三张中间表，中间表至少包含两个外键，分别关联两方主键
![[Pasted image 20240306201748.png]]
- *一对一*、
	- 多用于单表拆分，将一张表的基础字段放在一张表中，其他详细字段放另一张表
	- 实现：在任意一方加入外键，关联另外一方的主键，并且设外键为唯一的(UNIQUE)
![[Pasted image 20240306202313.png]]

### 进入多表查询阶段

> 在多张表中查询数据（笛卡尔积）

#### 连接查询

##### 内连接：相当于查询表之间的交集部分数据
- 隐式内连接
	- `SELECT 字段列表 FROM 表1,表2 WHERE 条件（外键）...;`
- 显示内连接
	- `SELECT 字段列表 FROM 表1 [INNER] JOIN 表2 ON 连接条件...`
##### 外连接：
- 左外连接：查询左表所有数据，以及两张表交集部分数据（常用这个）
	- `SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 连接条件`
	- 相当于查询表1的所有数据包含表1和表2交集部分的数据
- 右外连接：查询右表所有数据，以及两张表交集部分数据
	- `SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 连接条件`
	- 相当于查询表2（右表）的所有数据包含表1和表2交集部分的数据
##### 自连接：当前表与自身的连接查询，自连接必须使用表别名
- `SELECT 字段列表 FROM 表A 别名A JOIN 表A 别名B ON 条件...`
- `SELECT 字段列表 FROM 表A 别名A, 表B 别名B WHERE 条件`
- 自连接查询，可以是内连接查询，也可以是外连接查询

#### 连接查询-union，union all
```SQL
SELECT 字段列表 FROM 表A... UNION [ALL] SELECT 字段列表 FROM 表B...;

插叙的字段要是一样的
```
#### 子查询

> SQL语句中嵌套SELECT语句，称为嵌套查询，又称子查询

`SELECT * FROM t1 WHERE colum1 = (SELECT column FROM t2)`

- ***子查询外部的语句可以是INSERT/UPDATE/DELETE/SELECT的任何一个***
- 根据子查询位置，分为WHERE之后、FROM之后、SELECT之后

##### 标量子查询(子查询结果为单个值)
![[Pasted image 20240306220117.png]]
##### 列子查询(查询结果为一列)

![[Pasted image 20240306220732.png]]
##### 行子查询(子查询结果为一行)
![[Pasted image 20240306221826.png]]

##### 表子查询(子查询结果为多行多列)
![[Pasted image 20240306222208.png]]

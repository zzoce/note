

### MySQL高级课程简介

| 序号 | Day01              | Day02       | Day03          | Day04          |
| :--: | ------------------ | ----------- | -------------- | -------------- |
|  1   | Linux系统安装MySQL | 体系结构    | 应用优化       | MySQL 常用工具 |
|  2   | 索引               | 存储引擎    | 查询缓存优化   | MySQL 日志     |
|  3   | 视图               | 优化SQL步骤 | 内存管理及优化 | MySQL 主从复制 |
|  4   | 存储过程和函数     | 索引使用    | MySQL锁问题    | 综合案例       |
|  5   | 触发器             | SQL优化     | 常用SQL技巧    |                |



# 1. Linux 系统安装MySQL

### 1.1 安装MySQL

```
1). 在进行任何软件安装之前，请确保你的系统的软件包列表是最新的。打开终端并运行以下命令：
	
	sudo apt update
	
2). 在更新软件包列表后，这里我们可以查看一下可使用的MySQL安装包：
	
	sudo apt search mysql-server

3). 接下来可以使用以下命令安装MySQL服务器：
	
	# 安装最新版本
	sudo apt install -y mysql-server
	# 安装指定版本
	sudo apt install -y mysql-server-8.0
```

### 1.2 启动 MySQL 服务

```SQL
sudo service mysql start//启动

sudo service mysql stop//关闭

sudo service mysql status//查看mysql状态

sudo service mysql restart
```

### 1.3 设置MySQL

mysql 安装完成之后, 会自动生成一个随机的密码, 并且保存在一个密码文件中 

###### 登录MySQL
1. 查看默认账户和密码
```ubuntu
sudo cat /etc/mysql/debian.cnf
```
![[Pasted image 20240601193127.png]]
2. 使用默认账户密码登录
```
mysql -u debian-sys-maint -p
```
回车输入密码进入mysql
###### 修改密码
1. 在MySQL中输入命
```mysql
use mysql;
select user,plugin from user;
```

![[Pasted image 20240601194151.png]]

2. 修改root密码格式
```mysql
update user set plugin ='mysql_native_password' where user='root';//修改其密码格式
select user,plugin from user;//查询其用户
```
![[Pasted image 20240601194524.png]]
**==刷新权限==**
```mysql
flush privileges;
```

3. 修改密码策略
创建新用户密码非常的严格，长度要验证，还需要有特殊字符等规则，这样非常的麻烦，所以我们需要修改mysql的初始安全策略
```mysql
SHOW VARIABLES LIKE 'validate_password%';
```
![[Pasted image 20240601200226.png]]
修改策略
```mysql
set global validate_password_policy=LOW;
set global validate_password_length=6;
set global validate_password.mixed_case_count=0;
set global validate_password_special_char_count=0;
validate_password.length=6;
flush privileges;
```

4. 更改密码
```mysql
alter user 'root'@'localhost' identified by '新密码';
```

退出
```mysql
exit
```

5. 用新密码登录
```mysql
mysql -u root -p 
```
回车输入新密码


###### 设置远程访问
默认情况下，MySQL 数据库仅监听本地连接，如果想让外网远程连接到数据库，我们需要修改配置文件，让 MySQL 可以监听远程固定 ip 或者监听所有远程 ip。
```ubuntu
sudo code /etc/mysql/mysql.conf.d/mysqld.cnf
```
![[Pasted image 20240601202215.png]]
这个值是`127.0.0.1`的时候只监听本地连接，改成`0.0.0.0`可以监听所有连接，或者也可以改成仅允许指定ip连接都可以。

重启生效
```ubuntu
sudo service mysql restart
```


###### 允许root用户远程连接
mysql默认只允许root账号在本地使用，需要修改一下允许远程使用root账号（没试过其他账号的情况，但原理一致）。先登录mysql，执行一下命令
```mysql
use mysql;
select user, host from user;
```
![[Pasted image 20240601202453.png]]

`host`处为`localhost`时只允许本地使用，改成`%`即可远程使用：
```mysql
update user set host='%' where user='root';
FLUSH PRIVILEGES;//刷新权限
```

###### 关闭Ubuntu防火墙
1. 检查ubuntu自带的防火墙状态

```ubuntu
sudo ufw status
```

![image](https://img2022.cnblogs.com/blog/2711083/202206/2711083-20220617181358318-2012230118.png)

如果是`inactive`说明防火墙没开，那就不用管了。防火墙是干嘛的呢，我自己的理解就是，如果开了防火墙，那服务器上所有端口都是默认禁止连接的，只有你允许的端口才允许连接，类似于这种：  
![image](https://img2022.cnblogs.com/blog/2711083/202206/2711083-20220617183912241-565508796.png)

所以如果防火墙开了，那要么把防火墙直接关了：

```ubuntu
sudo ufw disable
```

要么添加一条规则让防火墙放行3306端口(mysql的默认端口)：

```ubuntu
sudo ufw allow 3306
```

# 2. 索引

#### 2.1 索引概述

MySQL官方对索引的定义为：索引（index）是帮助MySQL高效获取数据的数据结构（有序）。**在数据之外，数据库系统还维护者满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据， 这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。**如下面的==示意图==所示 : 

![[Pasted image 20240603120145.png]]

左边是数据表，一共有两列七条记录，最左边的是数据记录的物理地址（注意逻辑上相邻的记录在磁盘上也并不是一定物理相邻的）。为了加快Col2的查找，可以维护一个右边所示的<mark style="background: #FF5582A6;">二叉查找树</mark>，每个节点分别包含索引键值和一个指向对应数据记录物理地址的指针，这样就可以运用二叉查找快速获取到相应数据。

**==一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储在磁盘上。索引是数据库中用来提高性能的最常用的工具。==**



#### 2.2 索引优势劣势

优势

1. 类似于书籍的目录索引，提高数据检索的效率，降低数据库的IO成本。

2. 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗。

劣势

 1. 实际上索引也是一张表，该表中保存了主键与索引字段，并指向实体类的记录，所以索引列也是要占用空间的。

1. 虽然索引大大提高了查询效率，**同时却也降低更新表的速度（维护查找树）**，如对表进行INSERT、UPDATE、DELETE。因为更新表时，MySQL 不仅要保存数据，还要保存一下索引文件每次更新添加了索引列的字段，都会调整因为更新所带来的键值变化后的索引信息。



#### 2.3 索引结构

索引是在MySQL的存储引擎层中实现的，而不是在服务器层实现的。所以每种存储引擎的索引都不一定完全相同，也不是所有的存储引擎都支持所有的索引类型的。MySQL目前提供了以下4种索引：

- BTREE 索引 ： 最常见的索引类型，大部分索引都支持 B 树索引。
- HASH 索引：只有Memory引擎支持 ， 使用场景简单 。
- R-tree 索引（空间索引）：空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少，不做特别介绍。
- Full-text （全文索引） ：全文索引也是MyISAM的一个特殊索引类型，主要用于全文索引，InnoDB从Mysql5.6版本开始支持全文索引。

<center><b>MyISAM、InnoDB、Memory三种存储引擎对各种索引类型的支持</b></center>

| 索引        | InnoDB引擎  | MyISAM引擎 | Memory引擎 |
| --------- | --------- | -------- | -------- |
| BTREE索引   | 支持        | 支持       | 支持       |
| HASH 索引   | 不支持       | 不支持      | 支持       |
| R-tree 索引 | 不支持       | 支持       | 不支持      |
| Full-text | 5.6版本之后支持 | 支持       | 不支持      |
|           |           |          |          |

我们平常所说的索引，如果没有特别指明，都是指B+树（多路搜索树，并不一定是二叉的）结构组织的索引。其中聚集索引、复合索引、前缀索引、唯一索引默认都是使用 B+tree 索引，统称为 索引。



##### 2.3.1 BTREE 结构

BTree又叫多路平衡搜索树，一颗m叉的BTree特性如下：

- 树中每个节点最多包含m个孩子。
- 除根节点与叶子节点外，每个节点至少有$⌈m/2⌉$个孩子。
- 若根节点不是叶子节点，则至少有两个孩子。
- 所有的叶子节点都在同一层。
- 每个非叶子节点由n个key与n+1个指针组成，其中 $⌈m/2⌉<=n<=m-1$



以5叉BTree为例，key的数量：公式推导$⌈m/2-1⌉<= n <= m-1$。所以 $2 <= n <=4$ 。当n>4时，**==中间节点分裂到父节点，两边节点分裂==**。

插入 C N G A H E K Q M F W L T Z D P R X Y S 数据为例。

演变过程如下：

1). 插入前4个字母 C N G A 

![[Pasted image 20240603135211.png]] 

2). 插入H，n>4，中间元素G字母向上分裂到新的节点

![[Pasted image 20240603135223.png]] 

3). 插入E，K，Q不需要分裂

![[Pasted image 20240603135300.png]]

4). 插入M，中间元素M字母向上分裂到父节点G

![[Pasted image 20240603135338.png]] 

5). 插入F，W，L，T不需要分裂

![[Pasted image 20240603135400.png]] 

6). 插入Z，中间元素T向上分裂到父节点中 

![[Pasted image 20240603135421.png]] 

7). 插入D，中间元素D向上分裂到父节点中。然后插入P，R，X，Y不需要分裂

![[Pasted image 20240603135451.png]] 

8). 最后插入S，NPQR节点n>5，中间节点Q向上分裂，但分裂后父节点DGMT的n>5，中间节点M向上分裂

![[Pasted image 20240603135749.png]] 

到此，该BTREE树就已经构建完成了， BTREE树 和 二叉树 相比， 查询数据的效率更高， 因为对于相同的数据量来说，BTREE的层级结构比二叉树小，因此搜索速度快。



##### 2.3.3 B+TREE 结构

B+Tree为BTree的变种，B+Tree与BTree的区别为：

1. n叉B+Tree最多含有n个key，而BTree最多含有n-1个key。

2. **B+Tree的叶子节点保存所有的key信息，依key大小顺序排列。**

3. 所有的非叶子节点都可以看作是key的索引部分。

![[Pasted image 20240603135902.png]] 

由于B+Tree只有叶子节点保存key信息，查询任何key都要从root走到叶子。所以B+Tree的查询效率更加稳定。



##### 2.3.3 MySQL中的B+Tree

MySql索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，<mark style="background: #FF5582A6;">增加一个指向相邻叶子节点的链表指针</mark>，**==就形成了带有顺序指针的B+Tree，提高区间访问的性能==**。

MySQL中的 B+Tree 索引结构示意图: 

![[Pasted image 20240603140250.png]]  



#### 2.4 索引分类

1. 单值索引 ：即一个索引只包含单个列，一个表可以有多个单列索引

2. 唯一索引 ：索引列的值必须唯一，但允许有空值

3. 复合索引 ：即一个索引包含多个列



#### 2.5 索引语法

索引在创建表的时候，可以同时创建， 也可以随时增加新的索引。

准备环境: 

```SQL
//创建数据库
create database demo_01 default charset=utf8mb4;

//切换到创建的数据库
use demo_01;

//新建两张表
CREATE TABLE `city` (
  `city_id` int(11) NOT NULL AUTO_INCREMENT,
  `city_name` varchar(50) NOT NULL,
  `country_id` int(11) NOT NULL,
  PRIMARY KEY (`city_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `country` (
  `country_id` int(11) NOT NULL AUTO_INCREMENT,
  `country_name` varchar(100) NOT NULL,
  PRIMARY KEY (`country_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

//在表中插入数据
insert into `city` (`city_id`, `city_name`, `country_id`) values(1,'西安',1);
insert into `city` (`city_id`, `city_name`, `country_id`) values(2,'NewYork',2);
insert into `city` (`city_id`, `city_name`, `country_id`) values(3,'北京',1);
insert into `city` (`city_id`, `city_name`, `country_id`) values(4,'上海',1);

insert into `country` (`country_id`, `country_name`) values(1,'China');
insert into `country` (`country_id`, `country_name`) values(2,'America');
insert into `country` (`country_id`, `country_name`) values(3,'Japan');
insert into `country` (`country_id`, `country_name`) values(4,'UK');
```

<mark style="background: #FF5582A6;">查看数据库以及表的内容</mark>
```mysql
use demo_01;//切换到的数据库
show tables;//查看库中都有那些表
select * from city;//查看表的信息
```
![[Pasted image 20240603144247.png]]


##### 2.5.1 创建索引

语法 ： 	

```sql
CREATE 	[UNIQUE|FULLTEXT|SPATIAL]  INDEX index_name [USING  index_type] ON tbl_name(index_col_name,...)
//create 索引类型可以忽略（唯一，全文） index 索引名称 索引类型（默认btree）on 表名（字段名）


index_col_name : column_name[(length)][ASC | DESC]
```

示例 ： 为city表中的city_name字段创建索引 ；
```mysql
create index idx_city_name on city (city_name);
```
![[Pasted image 20240603144615.png]]    ​	  
**==创建多列索引==**

您还可以构建跨多列的索引。例如，假设您在数据库中已命名表的用户具有列_first_name_和_last_name_，和您经常访问使用这些列，那么你可以建立在两个列的索引在一起以提高性能的用户的记录，如下图：

```sql
CREATE INDEX user_name_idx ON users (first_name, last_name);
```

**提示：**您可以将数据库索引视为书籍的索引部分，以帮助您快速查找或定位书籍中的特定主题。
​	

##### 2.5.2 查看索引

语法： 

```
show index  from  table_name;
```

示例：查看city表中的索引信息；
```mysql
show index from city\G;//\G表示按列显示
```

![[Pasted image 20240603144806.png]] 
![[Pasted image 20240603145058.png]]
 



##### 2.5.3 删除索引

语法 ：

```mysql
DROP  INDEX  index_name  ON  tbl_name;
//           索引名称         表名称
```

示例 ： 想要删除city表上的索引idx_city_name，可以操作如下：
```mysql
drop index idx_city_name on city;
```

![[Pasted image 20240603145348.png]] 	 



##### 2.5.4 ALTER命令

```
1). alter  table  tb_name  add  primary  key(column_list); 

	该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL
	
2). alter  table  tb_name  add  unique index_name(column_list);
	
	唯一索引 ,这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）
	
3). alter  table  tb_name  add  index index_name(column_list);

	添加普通索引， 索引值可以出现多次。
	
4). alter  table  tb_name  add  fulltext  index_name(column_list);
	
	该语句指定了索引为FULLTEXT， 用于全文索引
	
```



#### 2.6 索引设计原则

​	索引的设计可以遵循一些已有的原则，创建索引的时候请尽量考虑符合这些原则，便于提升索引的使用效率，更高效的使用索引。

- **==对查询频次较高，且数据量比较大的表建立索引。==**

- 索引字段的选择，最佳候选列应当从where子句的条件中提取，如果where子句中的组合比较多，那么应当挑选最常用、过滤效果最好的列的组合。

- 使用唯一索引，区分度越高，使用索引的效率越高。

- 索引可以有效的提升查询数据的效率，**==但索引数量不是多多益善，索引越多，维护索引的代价自然也就水涨船高==**。对于插入、更新、删除等DML操作比较频繁的表来说，索引过多，会引入相当高的维护代价，降低DML操作的效率，增加相应操作的时间消耗。另外索引过多的话，MySQL也会犯选择困难病，虽然最终仍然会找到一个可用的索引，但无疑提高了选择的   代价。

- 使用短索引，索引创建之后也是使用硬盘来存储的，因此提升索引访问的I/O效率，也可以提升总体的访问效率。假如构成索引的字段总长度比较短，那么在给定大小的存储块内可以存储更多的索引值，相应的可以有效的提升MySQL访问索引的I/O效率。

- 利用最左前缀，N个列组合而成的组合索引，那么相当于是创建了N个索引，如果查询时where子句中使用了组成该索引的前几个字段，那么这条查询SQL可以利用组合索引来提升查询效率。

  ```
  创建复合索引:
  
  	CREATE INDEX idx_name_email_status ON tb_seller(NAME,email,STATUS);
  
  就相当于
  	对name 创建索引 ;
  	对name , email 创建了索引 ;
  	对name , email, status 创建了索引 ;
  ```


# 3. 视图

#### 3.1 视图概述

​	视图（View）是一种 **==虚拟存在的表==**。视图并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。通俗的讲，视图就是一条SELECT语句执行后返回的结果集。所以我们在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。

视图相对于普通的表的优势主要包括以下几项。

- 简单：使用视图的用户完全不需要关心后面对应的表的结构、关联条件和筛选条件，对用户来说已经是过滤好的复合条件的结果集。
- 安全：使用视图的用户只能访问他们被允许查询的结果集，对表的权限管理并不能限制到某个行某个列，但是通过视图就可以简单的实现。
- 数据独立：一旦视图的结构确定了，可以屏蔽表结构变化对用户的影响，源表增加列对视图没有影响；源表修改列名，则可以通过修改视图来解决，不会造成对访问者的影响。

#### 3.2 创建或者修改视图

创建视图的语法为：

```sql
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
//         替换

VIEW view_name [(column_list)]

AS select_statement

[WITH [CASCADED | LOCAL] CHECK OPTION]
```

修改视图的语法为：

```sql
ALTER [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]

VIEW view_name [(column_list)]

AS select_statement

[WITH [CASCADED | LOCAL] CHECK OPTION]
```

```
选项 : 
	WITH [CASCADED | LOCAL] CHECK OPTION 决定了是否允许更新数据使记录不再满足视图的条件。
	
	LOCAL ： 只要满足本视图的条件就可以更新。
	CASCADED ： 必须满足所有针对该视图的所有视图的条件才可以更新。 默认值.
```

示例 , 创建city_country_view视图 , 执行如下SQL : 

```sql
create or replace view city_country_view 
as 
select t.*,c.country_name from country c , city t where c.country_id = t.country_id;
```

查询视图 : 
```mysql
select * from city_country_view;
```

![[Pasted image 20240603154216.png]] 	



#### 3.3 查看视图

​	从 MySQL 5.1 版本开始，使用 SHOW TABLES 命令的时候不仅显示表的名字，同时也会显示视图的名字，而不存在单独显示视图的 SHOW VIEWS 命令。

![[Pasted image 20240603174131.png]]	 

同样，在使用 SHOW TABLE STATUS 命令的时候，不但可以显示表的信息，同时也可以显示视图的信息。	

![[Pasted image 20240603174414.png]] 

如果需要查询某个视图的定义，可以使用 SHOW CREATE VIEW 命令进行查看 ： 

![[Pasted image 20240603174632.png]]  

#### 3.4 删除视图

语法 : 

```sql
DROP VIEW [IF EXISTS] view_name [, view_name] ...[RESTRICT | CASCADE]	
//        如果存在
```

示例 , 删除视图city_country_view :

```
drop view if exists city_country_view ;
```
![[Pasted image 20240603174839.png]]


# 4. 存储过程和函数

#### 4.1 存储过程和函数概述

​	**==存储过程和函数是  事先经过编译并存储在数据库中的<mark style="background: #FF5582A6;">一段 SQL 语句的集合</mark>==**，调用存储过程和函数可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。	

​	存储过程和函数的区别在于函数必须有返回值，而存储过程没有。

​	函数 ： 是一个有返回值的过程 ；

​	过程 ： 是一个没有返回值的函数 ；

#### 4.2 创建存储过程

```sql
CREATE PROCEDURE procedure_name ([proc_parameter[,...]])
//                                参数
begin
	-- SQL语句
end ;
```



示例 ：

```sql 
delimiter $//将分割符（;）声明为$

create procedure pro_test1()
begin
	select 'Hello Mysql' ;//此时;并不表示结束
end$//结束语句，

delimiter ;//恢复分隔符
```
![[Pasted image 20240603200050.png]]



> [!NOTE] 知识小贴士
> `DELIMITER`
> 该关键字用来声明SQL语句的分隔符 , 告诉 MySQL 解释器，该段命令是否已经结束了，mysql是否可以执行了。默认情况下，delimiter是分号;。在命令行客户端中，如果有一行命令以分号结束，那么回车后，mysql将会执行该命令。

​	
#### 4.3 调用存储过程

```sql
call procedure_name() ;	
```
![[Pasted image 20240603200333.png]]
#### 4.4 查看存储过程

```sql
-- 查询db_name数据库中的所有的存储过程
select name from mysql.proc where db='db_name';//先版本mysql没有mysql.proc表


-- 查询存储过程的状态信息
show procedure status;


-- 查询某个存储过程的定义
show create procedure pro_test1 \G;
```

或者通过图形软件查看
![[Pasted image 20240603200956.png]]
#### 4.5 删除存储过程

```sql
DROP PROCEDURE  [IF EXISTS] sp_name ；
```
![[Pasted image 20240603201920.png]]
![[Pasted image 20240603202109.png]]

#### 4.6 语法

存储过程是可以编程的，意味着可以使用变量，表达式，控制结构 ， 来完成比较复杂的功能。

##### 4.6.1 变量

-  DECLARE

  通过 DECLARE 可以定义一个局部变量，该变量的作用范围只能在 BEGIN…END 块中。

```sql
DECLARE var_name[,...] type [DEFAULT value]
//      逗号分隔可一次生命多个变量  类型   默认值
```

示例 : 

```sql
 delimiter $

 create procedure pro_test2() 
 begin 
 	declare num int default 5;
 	select num+ 10; 
 end$

 delimiter ; 
```
![[Pasted image 20240603203320.png]]


- SET

直接赋值使用 SET，可以赋常量或者赋表达式，具体语法如下：

```
  SET var_name = expr [, var_name = expr] ...
```

示例 : 

```sql
  delimiter $
  
  create procedure  pro_test3()
  begin
  	declare name varchar(20);
  	set name = 'MYSQL';
  	select name ;
  end$
  
  delimiter ;
```
![[Pasted image 20240603204702.png]]


也可以通过select ... into 方式进行赋值操作 :

```SQL
delimiter  $

create procedure pro_test5()
begin
	declare  countnum int;
	select count(*) into countnum from city;//city表中的记录数
	select countnum;
end$

  delimiter ;
```
![[Pasted image 20240603205039.png]]


> [!NOTE] 只是小贴士
> **==存储过程和函数是  事先经过编译并存储在数据库中的<mark style="background: #FF5582A6;">一段 SQL 语句的集合</mark>==**
> 所以在city中记录数改变后，再调用这个过程，其结果也会改变



##### 4.6.2 if条件判断

语法结构 : 

```sql
if search_condition then statement_list
//  条件               满足就执行statement_list

	[elseif search_condition then statement_list] ...
	//      条件              满足就执行statement_list
	
	[else statement_list]
	//否则执行statement_list
	
end if;
```

需求： 

```
根据定义的身高变量，判定当前身高的所属的身材类型 

	180 及以上 ----------> 身材高挑

	170 - 180  ---------> 标准身材

	170 以下  ----------> 一般身材
```

示例 : 

```sql
delimiter $

create procedure pro_test6()
begin
  declare  height  int  default  175; //定义变量，设默认值
  declare  description  varchar(50);//定义字符串变量
  
  if  height >= 180  then
    set description = '身材高挑';
  elseif height >= 170 and height < 180  then
    set description = '标准身材';
  else
    set description = '一般身材';
  end if;
  select description ;
end$

delimiter ;
```

调用结果为 : 

![[Pasted image 20240603212457.png]]



##### 4.6.3 传递参数

语法格式 : 

```
create procedure procedure_name([in/out/inout] 参数名   参数类型)
//               存储过程名称     参数列表
...


IN :   该参数可以作为输入，也就是需要调用方传入值 , 默认
OUT:   该参数作为输出，也就是该参数可以作为返回值
INOUT: 既可以作为输入参数，也可以作为输出参数
```


 需求 :

```
根据传入的身高变量，获取当前身高的所属的身材类型  
```

示例:

```SQL 
create procedure pro_test5(in height int , out description varchar(100))
begin
  if height >= 180 then
    set description='身材高挑';
  elseif height >= 170 and height < 180 then
    set description='标准身材';
  else
    set description='一般身材';
  end if;
end$	
```

调用:

```
call pro_test5(168, @description)$

select @description$
```
![[Pasted image 20240603213640.png]]

> [!NOTE] 只是小贴士
> @description :  这种变量要在变量名称前面加上“@”符号，叫做用户会话变量，代表整个会话过程他都是有作用的，这个类似于全局变量一样。
> @@global.sort_buffer_size : 这种在变量前加上 "@@" 符号, 叫做 系统变量 






##### 4.6.4 case结构

语法结构 : 

```SQL
方式一 : 

CASE case_value
//    具体的值

  WHEN when_value THEN statement_list
  //    等于when_value 则执行statement_list
  
  [WHEN when_value THEN statement_list] ...
  
  [ELSE statement_list]
  //否则执行
  
END CASE;


方式二 : 

CASE

  WHEN search_condition THEN statement_list
  //   条件表达式，为真，则执行
  
  [WHEN search_condition THEN statement_list] ...
  
  [ELSE statement_list]
  
END CASE;

```

需求:

```
给定一个月份, 然后计算出所在的季度
```

示例  :

```sql
delimiter $

create procedure pro_test9(month int)
begin
  declare result varchar(20);
  case 
    when month >= 1 and month <=3 then 
      set result = '第一季度';
    when month >= 4 and month <=6 then 
      set result = '第二季度';
    when month >= 7 and month <=9 then 
      set result = '第三季度';
    when month >= 10 and month <=12 then 
      set result = '第四季度';
  end case;
  
  select concat('您输入的月份为 :', month , ' , 该月份为 : ' , result) as content ;
  
end$

delimiter ;
```

调用:
```sql
call pro_test9(10);
```
![[Pasted image 20240604123318.png]]


##### 4.6.5 while循环

语法结构: 

```sql
while search_condition do

	statement_list
	
end while;
```

需求:

```
计算从1加到n的值
```

示例  : 

```sql
delimiter $

create procedure pro_test8(n int)
begin
  declare total int default 0;
  declare num int default 1;
  while num<=n do
    set total = total + num;
	set num = num + 1;
  end while;
  select total;
end$

delimiter ;
```



##### 4.6.6 repeat结构

有条件的循环控制语句, **==当满足条件的时候退出循环==** 。while 是满足条件才执行，repeat 是满足条件就退出循环。

语法结构 : 

```SQL
repeat

  statement_list；//执行语句

  unttl search_condition//满足条件就退出，这里没有分号！！！！

end repeat;
```

需求: 

```
计算从1加到n的值
```

示例  : 

```sql
delimiter $

create procedure pro_test10(n int)
begin
  declare total int default 0;
  repeat 
    set total = total + n;
    set n = n - 1;
    until n=0  //这里没有分号！！！！
  end repeat;
  select total ;
end$

delimiter ;
```



##### 4.6.7 loop语句

LOOP 实现简单的循环，**==退出循环的条件需要使用其他的语句定义==**，通常可以使用 `LEAVE `语句实现，具体语法如下：

```sql
[begin_label:] LOOP
//可以在前面加一个别名

  statement_list

END LOOP [end_label]
```
如果不在 statement_list 中增加退出循环的语句，那么 LOOP 语句可以用来实现简单的死循环。

需求: 

```
计算从1加到n的值
```

示例  : 

```sql
delimiter $

create procedure pro_test10(n int)
begin
  declare total int default 0;
  c:loop  
    set total = total + n;
    set n = n - 1;
    if n<=0 then 
	    leave c;//退出loop，相当于break;
	end if;
  end loop c;
  select total ;
end$

delimiter ;
```


##### 4.6.8 leave语句

用来从标注的流程构造中退出，通常和 BEGIN ... END 或者循环一起使用。下面是一个使用 LOOP 和 LEAVE 的简单例子 , 退出循环：

```SQL
delimiter $

CREATE PROCEDURE pro_test11(n int)
BEGIN
  declare total int default 0;
  
  ins: LOOP
    
    IF n <= 0 then
      leave ins;
    END IF;
    
    set total = total + n;
    set n = n - 1;
  	
  END LOOP ins;
  
  select total;
END$

delimiter ;
```



##### 4.6.9 游标/光标

**==游标是用来存储查询结果集的数据类型 ,==** 在存储过程和函数中可以使用光标 **==对结果集进行循环的处理==**。光标的使用包括光标的声明、OPEN、FETCH 和 CLOSE，其语法分别如下。

声明光标：

```sql
DECLARE cursor_name CURSOR FOR select_statement ;
//declare emp_result cursor for select * from emp;
```

OPEN 光标：打开游标

```sql
OPEN cursor_name ;
```

FETCH 光标：使用游标遍历，抓取数据

```sql
FETCH cursor_name INTO var_name [, var_name] ...
//                     将数据赋值给后面的变量
```
![[Pasted image 20240604153743.png]]
CLOSE 光标：

```sql
CLOSE cursor_name ;
```



示例 : 

初始化脚本:

``` sql
create table emp(
  id int(11) not null auto_increment ,
  name varchar(50) not null comment '姓名',
  age int(11) comment '年龄',
  salary int(11) comment '薪水',
  primary key(`id`)
)engine=innodb default charset=utf8 ;

insert into emp(id,name,age,salary) values(null,'金毛狮王',55,3800),(null,'白眉鹰王',60,4000),(null,'青翼蝠王',38,2800),(null,'紫衫龙王',42,1800);

```



``` SQL
-- 查询emp表中数据, 并逐行获取进行展示
create procedure pro_test11()
begin
  declare e_id int(11);
  declare e_name varchar(50);
  declare e_age int(11);
  declare e_salary int(11);
  declare emp_result cursor for select * from emp;
  
  open emp_result;
  
  fetch emp_result into e_id,e_name,e_age,e_salary;
  select concat('id=',e_id , ', name=',e_name, ', age=', e_age, ', 薪资为: ',e_salary);
  
  fetch emp_result into e_id,e_name,e_age,e_salary;
  select concat('id=',e_id , ', name=',e_name, ', age=', e_age, ', 薪资为: ',e_salary);
  
  fetch emp_result into e_id,e_name,e_age,e_salary;
  select concat('id=',e_id , ', name=',e_name, ', age=', e_age, ', 薪资为: ',e_salary);
  
  fetch emp_result into e_id,e_name,e_age,e_salary;
  select concat('id=',e_id , ', name=',e_name, ', age=', e_age, ', 薪资为: ',e_salary);
  
  fetch emp_result into e_id,e_name,e_age,e_salary;//一共四个数据，第五次抓取会报错
  select concat('id=',e_id , ', name=',e_name, ', age=', e_age, ', 薪资为: ',e_salary);
  
  close emp_result;
end$

```



通过循环结构 , 获取游标中的数据 : 

```sql
DELIMITER $

create procedure pro_test12()
begin
  DECLARE id int(11);
  DECLARE name varchar(50);
  DECLARE age int(11);
  DECLARE salary int(11);
  DECLARE has_data int default 1;
  
  declare emp_result cursor for select * from emp;
  declare exit HANDLER for not found set has_data = 0;//这个声明，必须放在游标声明之后
  //not found 表示拿不到数据
  //https://www.cnblogs.com/datoubaba/archive/2012/06/20/2556428.html
  
  open emp_result;
  
  repeat//repeat循环，满足条件时结束循环
    fetch emp_result into id , name , age , salary;
    select concat('id为',id, ', name 为' ,name , ', age为 ' ,age , ', 薪水为: ', salary);
    until has_data = 0
  end repeat;
  
  close emp_result;
end$

DELIMITER ; 
```



#### 4.7 存储函数

语法结构:

``` 
CREATE FUNCTION function_name([param type ... ]) 
//              名字             参数列表
RETURNS type 
//返回值类型
BEGIN
	...
END;
```

案例 : 

定义一个存储过程, 请求满足条件的总记录数 ;

```SQL

delimiter $

create function count_city(countryId int)
returns int
begin
  declare cnum int ;

  //获取满足条件（city）的总记录数
  select count(*) into cnum from city where country_id = countryId;
  
  return cnum;
end$

delimiter ;
```

调用: 

```
select count_city(1);

select count_city(2);
```



# 5. 触发器

#### 5.1 介绍

触发器是与表有关的数据库对象，指 **==在 insert/update/delete 之前或之后，触发并执行触发器中定义的SQL语句集合==**。触发器的这种特性可以协助应用在数据库端确保数据的完整性 , 日志记录 , 数据校验等操作 。

使用别名 OLD 和 NEW 来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。**==现在触发器还只支持行级触发，不支持语句级触发。==**

| 触发器类型       | NEW 和 OLD的使用                      |
| ----------- | --------------------------------- |
| INSERT 型触发器 | NEW 表示将要或者已经新增的数据                 |
| UPDATE 型触发器 | OLD 表示修改之前的数据 , NEW 表示将要或已经修改后的数据 |
| DELETE 型触发器 | OLD 表示将要或者已经删除的数据                 |



#### 5.2 创建触发器

语法结构 : 

```sql
create trigger trigger_name 

before/after insert/update/delete

on tbl_name  

[ for each row ]  // 表示是  行级触发器

begin

	trigger_stmt ;

end;
```



示例 

需求

```
通过触发器记录 emp 表的数据变更日志 , 包含增加, 修改 , 删除 ;
```
![[Pasted image 20240604161846.png]]


首先创建一张日志表 : 

```sql
create table emp_logs(
  id int(11) not null auto_increment,
  operation varchar(20) not null comment '操作类型, insert/update/delete',
  operate_time datetime not null comment '操作时间',
  operate_id int(11) not null comment '操作表的ID',
  operate_params varchar(500) comment '操作参数',
  primary key(`id`)
)engine=innodb default charset=utf8;
```

![[Pasted image 20240604161946.png]]

创建 insert 型触发器，完成插入数据时的日志记录 : 

```sql
DELIMITER $

create trigger emp_logs_insert_trigger
after insert 
on emp 
for each row 
begin
  insert into emp_logs (id,operation,operate_time,operate_id,operate_params) values(null,'insert',now(),new.id,concat('插入后(id:',new.id,', name:',new.name,', age:',new.age,', salary:',new.salary,')'));	
  //(https://www.cainiaojc.com/sql/sql-ref-values.html)
end $

DELIMITER ;
```

创建 update 型触发器，完成更新数据时的日志记录 : 

``` sql
DELIMITER $

create trigger emp_logs_update_trigger
after update 
on emp 
for each row 
begin
  insert into emp_logs (id,operation,operate_time,operate_id,operate_params) values(null,'update',now(),new.id,concat('修改前(id:',old.id,', name:',old.name,', age:',old.age,', salary:',old.salary,') , 修改后(id',new.id, 'name:',new.name,', age:',new.age,', salary:',new.salary,')'));                                                                      
end $

DELIMITER ;
```

创建delete 行的触发器 , 完成删除数据时的日志记录 : 

```sql
DELIMITER $

create trigger emp_logs_delete_trigger
after delete 
on emp 
for each row 
begin
  insert into emp_logs (id,operation,operate_time,operate_id,operate_params) values(null,'delete',now(),old.id,concat('删除前(id:',old.id,', name:',old.name,', age:',old.age,', salary:',old.salary,')'));                                                                      
end $

DELIMITER ;
```



测试：

```sql
insert into emp(id,name,age,salary) values(null, '光明左使',30,3500);
insert into emp(id,name,age,salary) values(null, '光明右使',33,3200);

update emp set age = 39 where id = 3;

delete from emp where id = 5;
```



#### 5.3 删除触发器

语法结构 : 

```
drop trigger [schema_name.]trigger_name
```

如果没有指定 schema_name，默认为当前数据库 。

#### 5.4 查看触发器

可以通过执行 SHOW TRIGGERS 命令查看触发器的状态、语法等信息。

语法结构 ： 

```
show triggers ；
```

 




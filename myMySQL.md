# MySQL基础

![image-20221230150720513](https://typora---------image.oss-cn-beijing.aliyuncs.com/image-20221230150720513.png)

## MySQL概述

#### 数据库相关概念

| 名称           | 全称                                                         | 简称                             |
| -------------- | ------------------------------------------------------------ | -------------------------------- |
| 数据库         | 存储数据的仓库，数据是有组织的进行存储                       | DataBase(DB)                     |
| 数据库管理系统 | 操纵和管理数据库的大型软件                                   | DateBase Management System(DBMS) |
| SQL            | 操作**关系型数据库**的编程语言，定义了一套操作关系型数据库**统一**标准。 | Structured Query Language(SQL)   |

#### MySQL数据库

###### 启动与停止

**==win 搜索 cmd 以管理员身份运行==**

这个名字为mysql安装的时候注册的名字

![image-20221230154843473](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230154843473.png)

**==MySQL默认开机自启==**



###### 客户端连接

**方式一：MySQL提供的客户端命令行工具**

![image-20221230155801043](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230155801043.png)

![image-20221230155838067](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230155838067.png)

**方式二：系统自带的命令行工具执行指令**

![image-20221230160228487](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230160228487.png)

==注意：**使用这种方式时，需要配置PATH环境变量**==





###### 关系型数据库（RDBMS）

*使用表存储数据*

MySQL数据库就是关系型数据库

**概念：建立在关系模型基础上、由多张相互连接的==二维表==组成的数据库**

特点：

- **使用表存储数据**，**格式统一**，便于维护。
- 使用SQL语言操作，标准统一，使用方便。

![image-20221230202627796](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230202627796.png)

###### 数据模型

![image-20221230203154723](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230203154723.png)

**DBMS创建操作数据库**

客户段连接DBMS

## SQL

#### SQL通用语法及分类

###### SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾
2. SQL语句可以使用空格/缩进来增强语句的可读性
3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写
4. 注释：
   - 单行注释：--注释内容或         #注释内容（MySQL特有）
   - 多行注释：/\*注释内容/\*

###### SQL分类

![image-20230103111245840](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230103111245840.png)

#### DDL

**Data Definition Language**

###### 数据库操作

![image-20230104093245445](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104093245445.png)

**utf-8一个字符三个字节，数据库中有些字符是四个字节，所以字符集一般为utf-8mb4**



```sql
create database itheima default charset utf8mb4//指定字符集
```



###### 表操作

**==查询==**

![image-20230104093415662](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104093415662.png)

**==创建==**

![image-20230104104158130](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104104158130.png)

例子：

![image-20230104105106161](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104105106161.png)

![image-20230104105123267](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104105123267.png)

**==修改==**

![image-20230104112421101](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104112421101.png)

![image-20230104112652574](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104112652574.png)

![image-20230104112919744](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104112919744.png)

![image-20230104113003326](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104113003326.png)

![image-20230104114723636](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104114723636.png)

**重写创建的表是一个新表，原来表中的数据被删除。**

###### 数据类型

**==数值类型==**

![image-20230104105434292](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104105434292.png)

`age int unsigned//无符号的age`

**精度表示整个数据的长度，标度表示小数的位数**

![image-20230104110535441](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110535441.png)

**==字符串类型==**

![image-20230104110627032](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110627032.png)

**char是固定长度，括号中是多少就只能是多少。#长度不到，空格补全，大小固定**，定长

**varchar的括号中指定的是最长长度，因为每次存入时要计算实际的长度，所以性能较差。**变长

![image-20230104110855598](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110855598.png)



**==日期时间类型==**

![image-20230104111139056](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104111139056.png)

#### DML

**DML英文全称是Data  Manipulation Language（数据操作语言），用来对数据库中表的数据进行增删改操作。**

###### 添加数据（INSERT）

![image-20230104142449405](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104142449405.png)

**注意：**

- **插入数据时，指定的字段顺序需要与值的顺序是一一对应的。**
- **字符串和日期型数据应该包含在引号中。**
- **插入的数据大小，应该在字段的规定范围内。**

![image-20230104151344727](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104151344727.png)

###### 修改数据（UPDATA)

![image-20230104151834631](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104151834631.png)

**示范：**

![](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152052230.png)

###### 删除数据（DELETE）

![image-20230104152209770](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152209770.png)

**示范：**

![image-20230104152251801](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152251801.png)

#### DQL

**DQL英文全称是Data Query Language（数据查询语言），数据查询语言，用来查询数据库中表的记录。**

**==DQL编写顺序==**

![image-20230104152921859](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104152921859.png)

###### 基本查询

![image-20230104153040758](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104153040758.png)

**示例：**

![image-20230104154022336](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154022336.png)

![image-20230104154034224](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154034224.png)



![image-20230104154106550](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154106550.png)

![image-20230104154118384](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154118384.png)



![image-20230104154138951](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154138951.png)

![image-20230104154147859](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154147859.png)



![image-20230104154240003](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154240003.png)

![image-20230104154250694](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154250694.png)

###### 条件查询（WHERE）

![image-20230104154401966](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154401966.png)

![image-20230104154439641](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154439641.png)

![image-20230104154451124](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154451124.png)

**示范：**

![image-20230104154620167](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154620167.png)

![image-20230104154722630](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154722630.png)

![image-20230104154903802](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104154903802.png)

![image-20230104155026874](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155026874.png)

![image-20230104155253429](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155253429.png)

###### 聚合函数

**将一列数据作为一个整体，进行纵向计算**

**==常见的聚合函数==**

![image-20230104155407740](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155407740.png)

![image-20230104155442498](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104155442498.png)

**==null值不参与聚合函数运算==**

![image-20230104161533446](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104161533446.png)

![image-20230104161641355](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104161641355.png)

###### 分组查询（GROUP BY）

![image-20230104161914571](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104161914571.png)

![image-20230104162031618](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162031618.png)

![image-20230104162041067](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162041067.png)



![image-20230104162119984](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162119984.png)

![image-20230104162132771](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162132771.png)



![image-20230104162320948](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162320948.png)

![image-20230104162337921](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162337921.png)

![image-20230104162430898](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104162430898.png)

###### 排序查询

![image-20230104164812961](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164812961.png)

![image-20230104164835215](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164835215.png)

![image-20230104164854289](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164854289.png)



![image-20230104164938032](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164938032.png)

![image-20230104164954889](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104164954889.png)



![image-20230104165045970](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165045970.png)

![image-20230104165104238](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165104238.png)

###### DQL分页查询

![image-20230104165209152](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165209152.png)

![image-20230104165233298](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165233298.png)

![image-20230104165404509](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104165404509.png)

###### DQL执行顺序

![image-20230105101006669](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230105101006669.png)

#### DCL

DCL英文全称是**Data Control Language** （数据控制语言），用来**(1.)管理数据库用户，(2.)控制数据库的访问权限**。

###### 用户管理

![image-20230108102832634](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108102832634.png)

![image-20230108155559909](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155559909.png)

![image-20230108155724758](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155724758.png)

![image-20230108155829947](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155829947.png)

![image-20230108155920116](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108155920116.png)

###### 权限控制

![image-20230108160039908](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160039908.png)

![image-20230108160155187](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160155187.png)

![image-20230108160237824](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160237824.png)

![image-20230108160402682](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160402682.png)

![image-20230108160427559](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160427559.png)

![image-20230108160510730](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230108160510730.png)

**==*放在数据库名表示所有数据库，放在表名表示所有表==**





## 函数

**函数是指一段可以直接被另一段程序调用的程序或代码**

### 字符串函数

![image-20230109111708395](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230109111708395.png)

```sql
concat('hello','MySQL');
lower('hello');
upper('hello');
lpad('01',5,'-');
rpad('01',5,'-');
trim('  hello  ');
substring('hello MySQL',1,5);#索引是从1开始的!!!
```

```sql
#把企业员工的工号统一改为5位数，不足5位在前面补0，比如1号员工变为00001
update emp set workno = lpad(workno,5,'0');
```

### 数值函数

![截屏2024-04-16 16.39.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2016.39.12.png)

```sql
ceil(1.5);#2
floor(1.1);#1
mod(3,4);#3余数
rand();
round(2.345,2);#2.35
```

```sql
#通过数据库函数生成一个六位数验证码
select lpad(round(rand()*1000000,0),6,'0');
```

### 日期函数

![截屏2024-04-16 16.45.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2016.45.29.png)

```sql
curdate();#2021-10-11
curtime();#23:48:23
now();#2021-10-11 23:48:23

year(now());#2021
month(now());#10
day(now());#11

date_add(now(),interval 70 day);

datediff('2021-12-01','2023-10-01');#61第一个时间减去第二个时间，会出现负数

```

```sql
#查询所有员工的入职天数，并根据入职天数倒序排序
select name,datediff(curdate(),entrydate) as 'entryDays' from emp order by entryDays desc;
```

### 流程函数

![截屏2024-04-16 16.58.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2016.58.00.png)

```sql
if (true,'ok','error');

ifnull('ok','default');#第一个参数不为空返回第一个参数，否则返回第二个

#需求：查询emp表的员工姓名和工作地址(北京和上海-->一线城市，其他-->二线城市)
select name ,
	case workspace when '北京' then '一线城市' when '上海' then '一线城市' else '二线城市' end
	from emp;
	
	
#需求：展示学生的成绩 --- >= 85,展示优秀  >= 60展示及格 否则展示不及格
select 
	id,
	name,
	(case when math >= 85 then '优秀' when math >= 60 then '及格' else '不及格' end) '数学',
	english,
	chinese
from score;
```

## 约束

### 概述

**约束是作用于表中字段上的规则，用于限制存储在表中的数据**

**目的是保证数据库中的数据正确，有效性，完整性**

![截屏2024-04-16 17.11.06](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2017.11.06.png)

### 演示

```sql
create table user(
	id int primary key auto_increment comment '主键',
  name varchar(10) not null unique comment '姓名',
  age int check (age > 0 && age <= 120) comment '年龄'，
  status char(1) default '1' comment '状态',
  gander char(1) comment '性别'
) comment '用户表';
```

### 外键约束

#### 定义

**外键让两张表的数据之间建立连接，从而保证数据的一致性和完整性**



具有外键的表为**子表**(从表)，外键所关联的那张表为**父表**（主表）

不做数据库方面的外键约束，那么就只是**逻辑外键**

#### 添加外键

```sql
create table 表明(
	字段 数据类型,
	[constraint] [外键名称] foreign key (外键字段名) references 主表(主表列名)
);
```

```sql
alter table 表名 add constraint 外键名称 foreign key(外键字段名) references 主表(主表列名);
```

```sql
alter table emp add constraint fk_emp_dept_id foreigh key (dept_id) refereneces dept(id);
```

#### 删除外键

```sql
alter table 表名 drop foreign key 外键名称;
```

```sql
alter table emp drop foreign key fk_emp_dept_id;
```

#### 删除和跟新行为

![截屏2024-04-16 17.38.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2017.38.41.png)

![截屏2024-04-16 20.00.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.00.29.png)

## 多表查询

[MySQL --- 多表查询_mysql多表查询_Surpass余sheng军的博客-CSDN博客](https://blog.csdn.net/weixin_53622554/article/details/130642217?ops_request_misc=&request_id=&biz_id=102&utm_term=mysql多&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-130642217.142^v94^chatsearchT3_1&spm=1018.2226.3001.4187)

### 多表关系

![截屏2024-04-16 20.07.02](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.07.02.png)

**一对多**：在多的一方建立外键，指向一的一方的主键

**多对多**：

![截屏2024-04-16 20.08.46](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.08.46.png)

**一对一**：

![截屏2024-04-16 20.10.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.10.44.png)

### 多表查询概述

`select * from emp,dept;//会出现笛卡尔积`

笛卡尔积：在数学中，两个集合AB的所有组合情况---->在多表查询的时候，需要消除无效的笛卡尔积

![截屏2024-04-16 20.15.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.15.47.png)

`select * from emp,dept where emp.dept_id = dept.id//消除没用的数据`

![截屏2024-04-16 20.19.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.19.42.png)

### 内连接

**内连接查询两张表交集的部分**

#### 隐式内连接

`select 字段列表 from 表1 , 表2 where 条件...;    `

```sql
select * from emp,dept where emp.dept_id = dept.id
```

#### 显式内连接

```sql
select 字段列表 from 表1 [inner] join 表2 on 连接条件...;
```

**表起了别名后就不能再使用原来的名字了**(因为执行顺序中，先执行了from，已经把名字给改了)

![截屏2024-04-16 20.29.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2020.29.00.png)

### 外连接

#### 左外连接

```sql
select 字段列表 from 表1 left [outer] join 表2 on 条件...;
```

**相当于查询表1(左)的所有数据和包含表1表2交集部分的数据**

#### 右外连接

```sql
select 字段列表 from 表1 right [outer] join 表2 on 条件...;
```

**相当于查询表2(右)的所有数据和包含表1表2交集部分的数据**

*左外可以相互转换*

```sql
#查询emp表的所有数据和对应的部门信息
select e.*,d.name from emp e left outer join dept d on e.dept_id = d.id;
```

### 自连接

**一定要起别名!!!**

```sql
select 字段列表 from 表A 别名A join 表A 别名B on 条件...;
```

**自连接查询，可以是内连接查询，也可以是外连接查询**

```sql
#在公司员工总表中，查询员工及其所属领导的名字(领导和员工在同一张表中)
select a.name,b.name from emp a join emp b on a.managerid = b.id;#内连接
```

```sql
#查询所有员工，及其领导的名字，员工没有领带也要查出来
select a.name , b.name from emp a left outer join emp b on a.managerid = b.id;#左外连接
```

### 联合查询

**联合查询union查询，就是把多次查询的结果合并起来，形成一个新的查询结果集**

```sql
select 字段列表 from 表A...
union[all]
select 字段列表 from 表B...;
```

```sql
#查询薪资低于5k的员工和年龄大于50的员工全部查询出来
select * from emp where salary < 5000
union all#把all删掉的话，两个sql语句查到的重叠部分就没有了
select * from emp where age > 50
```

**对于联合查询的多张表的列数必须保持一致，字段类型也需要保持一致**

### 子查询

**sql语句中嵌套select语句，称为嵌套查询，又称为子查询**

```sql
select * from t1 where column1 = (select column1 from t2);
```

**子查询外部的语句可以是insert/update/delete/select中的任何一个**



#### 根据子查询结果

##### **标量子查询**

**(子查询结果为单个值)**

常用操作符**:= <> > >= < <=**

```sql
#查询销售部的所有员工信息
select * from emp where dept_id = (select id from dept where name = '销售部');
```

```sql
#查询在方东白入职之后的员工信息
select * from emp where entrydate > (select entrydate from emp where name = '方东白');
```

##### **列子查询**

**(子查询结果为一列)**

常用操作符：**in ,not in, any,some,all**

![截屏2024-04-16 21.25.12](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2021.25.12.png)

```sql
#查询销售部和市场部的所有员工信息
select * from emp where dept_id in (select id from dept where name = '销售部' or name = '市场部');
```

```sql
#查询比财务部所有人工资都高的员工信息
select * from emp where salary > all(select salary from emp where dept_id = (select id from dept where name = '财务部'));
```

```sql
#查询比研发部任意一人工资高的员工信息
select * from emp where salary > any(select salary from emp where dept_id = (select id from dept where name = '研发部'));
```

##### **行子查询**

**(子查询结果为一行)**

常用操作符: **= ,<>,in,not in**

```sql
#查询与张无忌的薪资及直系领导相同的员工信息
select * from emp where (salary,manegerid) = (select salary,managerid from emp where name = '张无忌');
```

##### **表子查询**

**(子查询结果为多行多列)**

常用**in**

```sql
#查询与路障课，宋远桥的职位和薪资相同的员工信息
select * from emp where (job,salary) in (select job,salary from emp where name = '路障课' or name = '宋远桥');
```

```sql
#查询入职日期是'2006-01-01'之后的员工信息及其部门信息
select e.*,d.* from (select * from emp where entrydate > '2006-01-01') e
left join dept d on e.dept_id = d.id;#左外连接
```

**根据子查询位置**分为

**where之后**

**from之后**

**select之后**

### 多表查询案例

![截屏2024-04-18 09.16.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2009.16.51.png)

### ![截屏2024-04-18 09.16.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2009.16.09.png)

```sql
#查询员工的姓名，年龄，职位，部门信息,(隐式内连接)
select e.name,e.age,e.job,d.name from emp e ,dept d where e.dept_id = d.id;

#查询年龄小于30岁的员工姓名，年龄，职位，部门信息,(显式内连接，on和where的使用,on后为两表的连接条件，where数据筛选)
select e.name,e.age,e.job,d.name from e inner join dept d on e.dept_id = d.id where e.age < 30;

#查询拥有员工的部门id，名称(去重)
select distinct d.id,d.name from emp e, dept d where e.dept_id = d.id;

#查询所有年纪大于40岁的员工，及其归属的部门名称，如果员工没有分配部门，也需要展示出来
select e.*,d.name from emp e left join dept d on e.dept_id = d.id where e.age > 40;

#查询所有员工的工资等级
select e.*,s.grade from emp e,salgrade s where e.salary >= s.losal and e.salary <= s.hisal;
select e.*,s.grade from emp e,salgrade s where e.salary between s.losal and s.hisal;

#查询研发部所有员工的信息及工资等级
select e.*,s.grade from (emp e,salgrade s ,dept d) where (e.salary between s.losal and s.hisal)
and (e.dept_id = d.id) and d.name = '研发部';

#查询研发部员工的平均工资
select avg(e.salary) from emp e,dept d where e.dept_id = d.id and d.name = '研发部';

#查询工资比灭绝搞的员工信息
select * from emp where salary > (select salary from emp where name = '灭绝');


#查询比平均工资薪资高的员工
select * from emp where salary > (select avg(e.salary) from emp);

#查询低于本部门平均工资的员工信息
select * from emp e2 where e2.salary < (select avg(e1.salary) from emp e1 where e1.dept_id = e2.dept_id);

#查询所有的部门信息，并统计部门的员工人数
select d.id,d.name,(select count(*) from emp e where e.dept_id = d.id) '人数' from dept d;

SELECT d.id, d.name, COUNT(e.id) AS '人数'  
FROM dept d  
LEFT JOIN emp e ON d.id = e.dept_id  
GROUP BY d.id, d.name;#group by

#查询所有员工的选课情况，展示出学生名称，学好和课程名称
select s.name,s.no,c.name from student s,student_cource sc,cource c where s.id = sc.studentid and sc.sourceid = c.id;
```

![截屏2024-04-18 09.50.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2009.50.29.png)

## 事物

### 事物简介

**事物是一组操作的集合，它是一个不可分隔的工作单位，事物会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作同时成功或者同时失败**

mysql默认是自动开始事物的，一次操作后直接提交了

### 事物操作

#### 查看/设置事物提交方式

```sql
select @@autocommit; 
set @@autocommit = 0; # 改为手动  
#会话参数，只针对当前窗口有效
```

#### 提交事物

```sql
commit;
```

#### 回滚事物

```sql
rollback;
```

#### 开启事物

**若不把自动提交改为手动**

**可以手动临时开启一段事物**

```sql
start transaction;
begin;
```

### 事物四大特性

**ACID**

| 特性                 | 说明                                                         |
| -------------------- | ------------------------------------------------------------ |
| 原子性 - Atomicity   | 事物是不可分隔的最小操作单元，要么全部成功，要么全部失败     |
| 一致性 - Consistency | 事物完成的时候，必须使得所有的数据都保持一致状态             |
| 隔离性 - Isolation   | 数据库系统提供的隔离机制，保证事物在不受外部并发操作影响的独立环境下运行(多个事物操作同一个数据库，相互独立不影响) |
| 持久性 - Durability  | 事物一旦回滚或者提交，它对数据库的改变就是永久的             |

### 并发事物问题

**多个并发事物在执行过程中出现的问题**

|    问题    | 描述                                                         |
| :--------: | ------------------------------------------------------------ |
|    脏读    | 一个事物读到另外一个事物还没有提交的数据                     |
| 不可重复读 | 一个事物先后读取同一条记录，但两次读取的数据不同(事物b提交后，事物a的两次读取的值就会不一样) |
|    幻读    | 一个事物按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了幻影，因为前两个问题解决了，事物a不断查询，没有对应的数据行，但是一旦插入，显示数据重复 |

![截屏2024-04-16 22.13.50](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.13.50.png)

![截屏2024-04-16 22.15.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.15.16.png)



![截屏2024-04-16 22.16.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.16.53.png)

### 事物隔离级别

![截屏2024-04-16 22.18.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-16%2022.18.19.png)

```sql
select @@transaction_isolation -- 查看事物隔离级别
set [session|global] transaction isolation level {read uncommitted | read committed | repeatable read | serializable} -- 设置事物隔离级别,一个时会话，一个是全部窗口
```

解决幻读的时候，事物a查询完比如id=4的数据，事物b插入id=4的数据会等待执行，直到事物a插入id=4的数据，并且提交，事物b那边结束运行，显示报错。

Serializable 是最高的事务隔离级别，在该级别下，事务串行化顺序执行，可以避免脏读、不可重复读与幻读。但是这种事务隔离级别效率低下，比较耗数据库性能，一般不使用，我的理解是事物串行化执行，一个事物开始执行操作了，就获得锁了，别人要等锁释放才可以执行操作，不然就是按下回车等待输出的过程。

**事物隔离级别越高，数据越安全，但是性能越差**



# 2.MySQL进阶

## 2.1存储引擎

### 2.1.1MySQL体系结构

![截屏2024-04-18 11.06.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.06.34.png)



从上到下依次为 **连接层，服务层，引擎层，存储层**



**index是在索引引擎层实现的，不同搜索引擎索引的实现是不一样的**

![截屏2024-04-18 11.10.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.10.20.png)

### 2.1.2存储引擎简介

**存储引擎就是存储数据，建立索引，跟新/查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型**(多张表可以选择不同的存储引擎)

![截屏2024-04-18 11.14.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.14.56.png)

**默认InnoDB**

```sql
#在创建表的时候，指定存储引擎
create table 表名(
  ...
)engine = INNODB [comment 注释];
```



```sql
#查看当前数据库支持的存储引擎
show engines;
```

![截屏2024-04-18 11.17.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.17.42.png)

### 2.1.3存储引擎特点

#### 2.1.3.1InnoDB

InnoDB是一种**兼容高可靠性和高性能**的通用存储引擎，在MySQL5.5之后，InnoDB是默认的MySQL存储引擎

- DML操作遵循ACID模型，支持**事物**

- **行级锁，**提高并发访问性能

- 支持**外键**FOREIGN KEY 约束，保证数据的完整性和正确性

- xxx.ibd,xxx为表名，innoDB引擎的每张表都会对应这样一个表空间文件，存储该表的表结构(frm,sdi)，数据和索引

  参数：innodb_file_pre_table(是否每一张表都有一个file，8版本后默认是打开的)

  

![截屏2024-04-18 11.31.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.31.39.png)

#### 2.1.3.2MyISAM

**MyISAM是mysql早期的默认存储引擎**

- 不支持事物，不支持外键
- 支持表锁，不支持行锁
- 访问速度快
- sdi，表结构信息
- myd，存放数据
- myi代表索引

![截屏2024-04-18 11.34.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.34.16.png)

#### 2.1.3.3Memory

**memory引擎的表数据存储在内存中，由于受到硬件问题，断电问题影响，只能将这些表作为临时表或者缓存使用**

- 内存存放，访问速度快
- 支持hash索引(默认)
- 文件：xxx.sdi 存储表结构信息

#### 2.1.3.4三者比较

**关键：事物，锁，外键**

![截屏2024-04-18 11.38.26](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.38.26.png)

### 2.1.4存储引擎选择

**选择存储引擎的时候，应该根据应用系统的特点选择合适的存储引擎，对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合**

![截屏2024-04-18 11.40.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2011.40.42.png)

## 2.2索引

### 2.2.1索引概述

索引(index) 是帮助MySQL高**效获取数据**的数据结构(**有序**)。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式**引用(指向)数据**，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引.

**无索引-->全表扫描，效率低**



**优点**

- 提高数据检索的效率，降低数据库的IO成本
- 通过索引列对数据进行排序，降低数据排序的成本，降低CPU的消耗

**缺点**

- 索引列也要占用空间
- 索引大大提高了查询的效率，同时也降低跟新表的速度，如对表进行INSERT，UPDATE，DELETE，效率低

### 2.2.2索引结构

**不同引擎结构不同**

![截屏2024-04-18 15.14.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.14.17.png)

![截屏2024-04-18 15.15.15](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.15.15.png)

<u>默认B+tree结构</u>

#### 2.2.2.1B-tree

**二叉树缺点**：顺序插入的时候，会形成一个链表，查询性能大大降低，大数据量情况下，层级较深，减速速度慢

**红黑树**：大数据情况下，层级较深，减速速度慢

**B-Tree：多路平衡查找树**

树的度---一个节点的子节点个数

一颗最大度数(max-degree) 为5(5阶) 的b-tree为例(每个节点最多存储4key，5个指针)

![截屏2024-04-18 15.22.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.22.08.png)

这个数据结构在插入数据的时候，先根据值插入到某一个数据单元中去，当加入后单元的元素个数达到了临界值（阶数）则**中间元素，向上分裂**

![截屏2024-04-18 15.29.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.29.19.png)



![截屏2024-04-18 15.29.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.29.34.png)



#### 2.2.2.2B+tree

**以一颗最大度数(max-degree)为4(4阶) 的b+tree为例：**

所有元素都会出现在叶子节点，并且形成一个单向链表

上面的数字充当索引使用

![截屏2024-04-18 15.32.17](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.32.17.png)

![截屏2024-04-18 15.35.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.35.16.png)

![截屏2024-04-18 15.35.25](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.35.25.png)

**MySQL索引数据结构对经典的B+Tree进行了优化，在原B+Tree的基础上，增加了一个指向相邻叶子节点的链表指针，形成了带有顺序指针的B+Tree，提高区间访问的性能**

![截屏2024-04-18 15.37.44](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.37.44.png)

##### 2.2.2.2.1优点

**相对于二叉树，层级更少，搜索效率更高**

**对于B-tree，无论是叶子节点还是非叶子节点，都会保存数据，这样导致一页中存储的键值对减少，指针跟着减少，要同样保存大量数据，只能增加树的高度，导致性能降低**，b+值都在叶子节点，**搜索效率稳定**，双向链表的设计使得**范围搜索和排序高效**

**相对于Hash索引，B+tree支持范围匹配及排序操作**

#### 2.2.2.3Hash

**哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中**

![截屏2024-04-18 15.42.01](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.42.01.png)

- **Hash索引只能用于对等比较(=,in) 不支持范围查询(between,>,<,...)**
- **无法利用索引完成排序操作**
- **查询效率高，通常只需要一次检索就可以了，效率通常高于B+tree索引**

**在MySQL中，支持hash索引的是Memory引擎，而InnoDB中具有自适应hash功能(在指定条件下自动构建hash)，hash索引是存储引擎根据B+tree索引在指定条件下自动构成的**

### 2.2.3索引分类

![截屏2024-04-18 15.52.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.52.43.png)



#### 2.2.3.1InnoDB分类

**在InnoDB索引引擎中，根据索引的存储形式，又可以分为以下两种**

![截屏2024-04-18 15.54.47](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2015.54.47.png)

**二级索引--辅助索引**

聚集索引选取规则：

**如果存在主键，主键索引就是聚集索引**

**如果不存在主键，将使用第一个唯一(UNIQUE)索引作为聚集索引**

**如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引**

![截屏2024-04-18 16.01.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2016.01.00.png)

比如按照name进行查询的时候，先在name中找到对应的主键id，再在聚集索引中找到row行，这个过程称为**回表查询**

#### 2.2.3.2InnoDB主键树高度

**InnoDB主键索引的B+tree高度为多高**

假设：

**一页大小为16k，一行数据大小为1k，一页中可以存储16行这样的数据，InnoDB的指针占用6个字节的空间(固定)，主键若为bigiht，占用字节为8**

在高度为2的情况下，有以下等式：

第一页数据：

`n * 8 + (n + 1 * 6 = 16 * 1024) --> n = 1170`

那么一共可以有1171个指针指向叶子节点，每个叶子节点大概可以有16行数据，

那么可以存储的数据行数为 1171 * 16 = 18736

高度为3:

**1171 * 1171 * 16 = 21939856**

### 2.2.4索引语法

#### 2.2.4.1创建索引

`create [unique|fulltext] index index_name on table_name (index_col_name...);`

**不指定[]为常规索引**

**一个为单列索引，多个为联合索引**

#### 2.2.4.2查看索引

`show index from table_name;`

#### 2.2.4.3删除索引

`drop index index_name on table_name`

![截屏2024-04-18 16.28.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2016.28.45.png)

### 2.2.5SQL性能分析

#### 2.2.5.1查看执行频次

**MySQL客户端连接成功后，通过show [session|global] status 命令可以提供服务器状态信息。通过如下指令，可以查看到当前数据库的INSERT，UPDATE，DELETE，SELECT的访问频次**(全局和会话)

`show global status like 'COM___';`

![截屏2024-04-18 16.34.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2016.34.19.png)

#### 2.2.5.2慢查询日志

**慢查询日志记录了所有执行时间超过指定参数(long_query_time,单位:s,默认10秒)的所有sql语句的日志**

**MySQL的慢查询日志默认没有开启，需要在MySQL的配置文件(/etc/my.cnf)配置信息：**

```
#慢查询日志
slow_query_log=1
long_query_time=2
```

```
cd /var/lib/mysql
localhost-slow.log
```

![截屏2024-04-18 20.52.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2020.52.24.png)

![截屏2024-04-18 20.55.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2020.55.49.png)

#### 2.2.5.3show profiles

**profile 详情**

**show profiles 能够在做sql优化的时候帮我们了解时间都耗费到哪里去了，通过have_profiling参数，能够看到当前mysql是否支持profile操作**

![截屏2024-04-18 21.16.42](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2021.16.42.png)

默认profiling是关闭的，可以通过set语句在session/global级别开启profiling

```
set profiling = 1;
```

![截屏2024-04-18 21.18.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2021.18.53.png)

```sql
#查看每一条SQL的耗时基本情况
show profiles;

#查看指定query_id的SQL语句各个阶段的耗时情况
show profile for query query_id;

#查看指定query_id的SQL语句CPU的使用情况
show profile cpu for query query_id; ##query_id是show中看到的 
```

#### 2.2.5.4explain

```sql
#直接在select语句之前加上关键字explain/desc
EXPLAIN SELECT 字段列表 from 表名 WHERE 条件;
```

![截屏2024-04-18 21.32.34](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-18%2021.32.34.png)

- **id** - select查询的序列号，表示查询中执行select字句或者是操作表的顺序(id相同，执行顺序从上到下；id不同，值越大，越先执行,多表查询的时候会有多个id)

- **select_type** - 表示select的类型，常见的取值有*<u>simple</u>*(简单表，即不使用表连接或子查询)，*<u>primary</u>*(主查询，即外层的查询)，<u>union</u>（*union*中的第二个或者后面的查询语句），*<u>subquery</u>*(select/where之后包含了子查询)等

- **type** - 表示连接类型，性能由好到差的连接类型为**NULL,system,const,eq_ref,ref,range,index,all**,     null基本达不到，到const就很好了，不查询任何表的时候，直接的select才可以达到null，业务系统的sql一般不会到null  

  主键和唯一索引回到const  

  非唯一索引会出现ref

  all-全表扫描

  index-用了索引，但还是会对索引进行扫描遍历

- **possible_key** - 显示可能应用在这张表上的索引，一个或者多个

- **key** - 实际使用的索引如果为null，则没有使用索引

- **key_len** - 表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好

- **rows** - MySQL认为必须要执行查询的行数，在innoDB引擎表中，是一个估计值，可能不总是准确的

- **filiterd** - 表示返回结果的行数占需读取行数的百分比，filiterd的值越大越好

- **extra** - 额外信息

**type的range**

![截屏2024-04-19 11.57.00](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.57.00.png)



### 2.2.6索引使用

#### 2.2.6.1最左前缀法则

如果索引了多列(**联合索引)**,要遵循最左前缀法则。最左前缀法则指的是**查询从索引的最左列开始**，并且不跳过索引中的列

如果跳过了某一列，索引将部分失效(**后面的字符索引失效**)

![截屏2024-04-19 11.09.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.09.08.png)

```sql
#假设上述的profession，age，status在同一个联合索引里面，那么查询这个字段的时候，要使用索引，会从左向右匹配，那么最左边的匹配一定为联合索引的最左的值，向右匹配的过程中直到不满足联合索引的顺序，结束索引查找，当一次索引都用不上，变成全表查询(all)

#如果用了一点索引，之后再做匹配，index  ,  用了那个索引条件就到头了 -- ref

#只要字段存在，顺序不重要，and会做重排序
```

![截屏2024-04-19 11.45.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.45.56.png)

#### 2.2.6.2索引失效情况

##### 2.2.6.2.1范围查询

**联合索引中，出现范围查询(>,<),范围查询右侧的列索引失效**

![截屏2024-04-19 11.18.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.18.45.png)

```
>= 会比 > 使用更多索引，性能更高
```

##### 2.2.6.2.2索引列运算

**在索引列上进行运算操作，索引将失效**

![截屏2024-04-19 11.42.24](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.42.24.png)

![截屏2024-04-19 11.52.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.52.29.png)

##### 2.2.6.2.3字符串不加引号

**字符串类型字段使用时，不加引号，索引将失效，会产生隐式类型转换**

![截屏2024-04-19 11.49.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.49.40.png)

![截屏2024-04-19 11.50.37](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.50.37.png)

##### 2.2.6.2.4模糊匹配

**如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效**

![截屏2024-04-19 11.53.53](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.53.53.png)

![截屏2024-04-19 11.55.22](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2011.55.22.png)

##### 2.2.6.2.5or连接的条件

**用or分隔开的条件，如果or前的条件中的列有索引，而后面的列中没有索引，那么涉及的索引都不会被用到**

![截屏2024-04-19 12.01.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2012.01.19.png)

##### 2.2.6.2.6数据分布影响

**如果mysql评估使用索引比全表更慢，则不适用索引**

![截屏2024-04-19 12.05.54](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2012.05.54.png)

**当mysql认为大多数数据满足你的条件就不会走索引**

#### 2.2.6.3sql提示

**在sql语句中加入一些认为的提示来达到优化操作的目的**

比如说，一个字段既是联合索引的最左字段又是一个单独的单列索引，此时看使用哪一个索引

```sql
#use index --> 建议 有可能会不适用
explain select * from tb_user use index(idx_user_pro) where profession = 'aa';

#ignore index
explain select * from tb_user ignore index(idx_user_pro) where profession = 'aa';

#force index
explain select * from tb_user force index(idx_user_pro) where profession = 'aa';
```

#### 2.2.6.4覆盖索引

**尽量使用索引覆盖（查询使用了索引，并且需要返回的列，在该索引中已经全部能够找到），减少select***

| extra                    | description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| using index condition    | 查找使用了索引，但是需要回表返回数据                         |
| using where; using index | 查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据 |

![截屏2024-04-19 16.04.16](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.04.16.png)

#### 2.2.6.5前缀索引

**当字段类型为字符串（varchar text 等）有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费大量的磁盘IO，影响查询效率，此时可以只将字符串的一部分前缀，建立索引，这样可以大大节约索引空间，从而提高索引效率**

```sql
create index idx_xxx on table_name(column(n)); #提取列的前n个字符
```

**前缀长度**

**可以根据索引的选择性来决定，而选择性是指不重复的索引值(基数)和数据表的记录总数的比值，索引选择性越高则查询效率越高，**

**唯一索引的选择性为1，这是最好的索引选择性，性能也是最好的**

```sql
select count(distinct email) / count(*) from tb_user; #distinct对数据去重
select count(distinct substring(email,1,5)) / count(*) from tb_user;
```

![截屏2024-04-19 16.36.08](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.36.08.png)

到了索引可以判断完的地方还没结束，根据row要再检查前缀后是否符合条件(叶子是链表),如上述的17799

#### 2.2.6.6单列索引和联合索引

![截屏2024-04-19 16.40.19](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.40.19.png)

**phone和name都是一个单列索引，但是在查找的时候，只使用了phone的单列索引，name是通过回表查询得到的**

![截屏2024-04-19 16.42.32](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.42.32.png)

**在业务场景中，如果存在多个查询条件，考虑针对查询字段建立索引时，建议建立联合索引，而非单列索引**

<u>*联合索引的结构和使用情况*</u>

![截屏2024-04-19 16.44.52](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.44.52.png)



### 2.2.7索引设计原则

![截屏2024-04-19 16.48.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2016.48.55.png)

**注意第7条**

## 2.3SQL优化

### 2.3.1插入数据

#### 2.3.1.1批量插入

**500～1000条**

```
insert into table values (),(),();
```

#### 2.3.1.2手动事物提交

**默认自动事物提交，多次提交会有频繁的事物开启和关闭**

```sql
start transaction;
insert into table values (),(),();
insert into table values (),(),();
insert into table values (),(),();
commit;
```

#### 2.3.1.3主键顺序插入

![截屏2024-04-19 17.04.41](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.04.41.png)

**按照主键的顺序插入效率更高**

#### 2.3.1.4大批量插入数据

**如果一次性需要插入大批量数据，使用insert语句插入性能比较低，此时可以使用mysql数据库提供的load指令进行插入**

![截屏2024-04-19 17.10.49](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.10.49.png)

![截屏2024-04-19 17.11.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.11.39.png)



### 2.3.2主键优化

#### 2.3.2.1数据组织方式

在InnoDB存储引擎中，**表数据都是根据主键顺序组织存放的**，这种存储方式的表称为索引组织表(index organized table **IOT**)

#### 2.3.2.2页分裂

**页可以为空，也可以填充一般，也可以填满。**

**每个页包含了2-N行数据(如果一行数据过大，会行溢出)，根据主键排序**

![截屏2024-04-19 17.26.39](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2017.26.39.png)

![截屏2024-04-19 18.11.11](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.11.11.png)

#### 2.3.2.3页合并

**当删除一行记录时，实际上记录并没有被物理删除，只是被标记(flaged)为删除，并且它的空间变得允许被其他记录声明使用**

![截屏2024-04-19 18.14.27](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.14.27.png)

![截屏2024-04-19 18.15.04](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.15.04.png)

**MERGE_THRESHOLD:合并页的阈值，可以自己设置，在创建表或者创建索引时指定**

#### 2.3.2.4主键设计原则

**满足业务需求的情况下，尽量降低主键的长度**(聚集索引只有一个，但是二级索引会有很多个，主键过长，可能会导致二级索引的数据域占用空间过大)

**插入数据时，尽量选择顺序插入，选择使用auto_increment自增主键**(顺序插入效率高)

**尽量不要使用UUID做主键或者是其他自然主键，如身份证号**(无序乱序插入，可能出现页分裂现象,而且长度可能过长)

**业务操作的时候，避免对主键的修改**（会变跟索引结构）

### 2.3.3order by优化

- **Using filesort** : 通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序都叫FileSort排序
- **Using index** ： 通过有序索引扫描直接返回有序数据，这种情况为using index，不需要额外排序，**操作效率高**

![截屏2024-04-19 18.26.51](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.26.51.png)

<u>*为age添加索引后*</u>

![截屏2024-04-19 18.27.43](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.27.43.png)

**违背最左前缀法则**

![截屏2024-04-19 18.30.09](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.30.09.png)

**与索引的排序方式不一致**

![截屏2024-04-19 18.31.21](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.31.21.png)

**为特殊的字段排序创造特有的索引规则，使得filesort不出现**

![截屏2024-04-19 18.33.56](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.33.56.png)

![截屏2024-04-19 18.34.29](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.34.29.png)

**以上优化的前提为----覆盖索引**

![截屏2024-04-19 18.38.20](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.38.20.png)

### 2.3.4group by优化

**分组字段没有创建索引之前**

![截屏2024-04-19 18.41.14](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.41.14.png)

**创建索引后**

![截屏2024-04-19 18.42.40](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.42.40.png)

**关于最左前缀法则**

![截屏2024-04-19 18.43.45](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.43.45.png)

**条件构造后满足最左前缀法则**

![截屏2024-04-19 18.44.55](https://typora---------image.oss-cn-beijing.aliyuncs.com/%E6%88%AA%E5%B1%8F2024-04-19%2018.44.55.png)

### 2.3.5limit优化

**覆盖索引 + 子查询**

```sql
#速度慢，数据冗余
select * from tb_sku limit 9000000,10; 

#in limit   mysql版本不支持limit和in/all/any/some子查询
select * from tb_sku where id in (select id from tb_sku order by id limit 9000000,10);

#res
select s.* from tb_sku s , (select id from tb_sku order by id limit 9000000,10) a where s.id = a.id;
```

### 2.3.6count优化

**MyISAM引擎把一个表的总行数存在了磁盘上，因此执行count(*)的时候会直接返回个数，效率高（前提没有where）**

**InnoDB引擎执行的时候，需要把数据一行一行地从引擎里面读出来，然后累计计数**

优化思路：**自己计数**

`count()是一个聚合函数，对于返回的结果集，一行行判断，如果count函数的参数不是null，累计值+1，否则不加，最后返回累计值`

*用法：*

```sql
count(*)；#求这个表的总记录数量，InnoDB不会把全部字段取出来，而是做了专门优化，不取值，服务层直接按行进行累加
count(主键)；#遍历整张表，取主键的id，返回给服务层，服务层拿到主键后，直接按行进行累加(主键不可能为null)
count(字段)；#不是null的值(InnoDB引擎遍历整张表把每一行的字段取出来，返回给服务层,根据有没有not null 约束来进行直接累计计数或先判断再累➕计数)
count(1)；#遍历整张表，但不取值，服务层对于返回的每一行，放一个数字1进去，直接按行进行累加
```

效率排行：

`count(字段) < count(主键id)  < count(1) 约等于 count(*),所以尽量使用count(*)`

### 2.3.7update优化

事物开启后，使用update跟新数据的时，一定要**进行索引字段当where条**件，不然就会触发**表锁**，别的事物无法操作

**并发性能降低**，使用索引字段当where条件只会触发行锁(InnoDB特点)

**InnoDB的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行锁升级为表锁**

## 2.4视图/存储过程/触发器



## 2.5锁



## 2.6InnoDB引擎



## 2.7MySQL管理

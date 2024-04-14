# MySQL数据库

![image-20221230150720513](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230150720513.png)

## MySQL概述

#### 数据库相关概念

| 名称           | 全称                                                         | 简称                             |
| -------------- | ------------------------------------------------------------ | -------------------------------- |
| 数据库         | 存储数据的仓库，数据是有组织的进行存储                       | DataBase(DB)                     |
| 数据库管理系统 | 操纵和管理数据库的大型软件                                   | DateBase Management System(DBMS) |
| SQL            | 操作关系型数据库的编程语言，定义了一套操作关系型数据库统一标准。 | Structured Query Language(SQL)   |

#### MySQL数据库

###### 启动与停止

**==win 搜索 cmd 以管理员身份运行==**

![image-20221230154843473](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230154843473.png)

**==MySQL默认开机自启==**

###### 客户端连接

**方式一：MySQL提供的客户端命令行工具**

![image-20221230155801043](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230155801043.png)

![image-20221230155838067](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230155838067.png)

**方式二：系统自带的命令行工具执行指令**

![image-20221230160228487](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230160228487.png)

==注意：使用这种方式时，需要配置PATH环境变量==

###### 关系型数据库（RDBMS）

MySQL数据库就是关系型数据库

**概念：建立在关系模型基础上、由多张相互连接的==二维表==组成的数据库**

特点：

- 使用表存储数据，格式统一，便于维护。
- 使用SQL语言操作，标准统一，使用方便。

![image-20221230202627796](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230202627796.png)

###### 数据模型

![image-20221230203154723](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20221230203154723.png)

## SQL

#### SQL通用语法及分类

###### SQL通用语法

1. SQL语句可以单行或多行书写，以分号结尾
2. SQL语句可以使用空格/缩进来增强语句的可读性
3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写
4. 注释：
   - 单行注释：--注释内容或#注释内容（MySQL特有）
   - 多行注释：/\*注释内容/\*

###### SQL分类

![image-20230103111245840](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230103111245840.png)

#### DDL

###### 数据库操作

![image-20230104093245445](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104093245445.png)

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

重写创建的表是一个新表，原来表中的数据被删除。

###### 数据类型

**==数值类型==**

![image-20230104105434292](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104105434292.png)

**精度表示整个数据的长度，标度表示小数的位数**

![image-20230104110535441](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110535441.png)

**==字符串类型==**

![image-20230104110627032](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230104110627032.png)

char是固定长度，括号中是多少就只能是多少。

varchar的括号中指定的是最长长度，因为每次存入时要计算实际的长度，所以性能较差。

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

DCL英文全称是Data Control Language （数据控制语言），用来**管理数据库用户，控制数据库的访问权限**。

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

#### 函数

**函数是指一段可以直接被另一段程序调用的程序或代码**

###### 字符串函数

![image-20230109111708395](https://wangmingjie1.oss-cn-zhangjiakou.aliyuncs.com/image-20230109111708395.png)

#### 多表查询

[MySQL --- 多表查询_mysql多表查询_Surpass余sheng军的博客-CSDN博客](https://blog.csdn.net/weixin_53622554/article/details/130642217?ops_request_misc=&request_id=&biz_id=102&utm_term=mysql多&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-130642217.142^v94^chatsearchT3_1&spm=1018.2226.3001.4187)

### 功能：SQL
语法：<SQL>  [数据库 <数据库连接名>] [DQL]
参数：**SQL**
数据库的结构化查询语言。必要参数；字串类型；必须省略参数名。
参数：** 数据库**
对应NLC配置项里的数据库连接名，配置包括url、驱动、用户名、口令等必要的连接信息。非必要参数，缺省是第一个数据库连接名；类型是字串，含结果为字串的计算式；必须省略参数名。
>例子：根据连接名orcl访问数据库, 执行SQL，查询订单例子表并生成相应的结构化数据。
NLC：SQL "select * from Orders where Amount量>1000 and Amount量<=10000"; orcl
结果：
OrderID	ClientID	SellerId	Amount量	OrderDate
1	WVF	5	1440.0	2022-01-04
2	UFS	13	1863.4	2022-01-08
4	JFS	27	1670.8	2022-01-12
5	DSG	15	3730.0	2022-01-15
6	JFE	10	1444.8	2022-01-19
参数：**DQL**
表示参数**SQL**使用了DQL语法。DQL是一种类似SQL的语法，具体语法你不用管。非必要参数；布尔类型；参数名不能省略，没有参数值。
>例子：执行某个DQL
NLC：SQL "SELECT SUM(buss_zwsc), AVG(fee_rate) ON dim_time, dim_company FROM fact_package"; DQL  
解释：上面NLC代码省略了数据源名。

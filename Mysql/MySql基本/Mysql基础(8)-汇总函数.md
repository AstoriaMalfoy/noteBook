# ==汇总函数==
## 聚集函数
SQL中有时候是需要数据的统计信息,对于数据是不需要的.为此SQL提供了专门的函数,使用这些函数,SQL可以对数据进行检索,以便与分析和报表的生成.常见的例子是有
* 确定表中数据行数
* 获得表中某些数据的和
* 找出列表的最大值,最小值,平均值.
  对于上述这种需求中,需要的都是汇总表中的数据,而不需要数据本身,因此在进行统计的时候是不需要返回数据的,为了便于这种信息的检索,SQL提供了5个聚集函数,这些函数可以进行上述检索,并且这种函数在各个DBMS中得到了相当一致的支持.
  **聚集函数:对某些行进行计算的函数,计算并返回一个值**
  SQL聚集函数表

|函数|说明|
|:--|:--|
|AVG()|返回某列的平均值|
|COUNT()|返回某列的行数|
|MAX()|返回某列的最大值|
|MIN()|返回某列的最小值|
|SUM()|返回某列之和|

**AVG()函数**
AVG函数通过对表中相关行计算某一列的和,求的该列的平均值,AVG()可以用来返回所有列的平均值,也可以返回特定行或者列的平均值.
*eg.*
`select AVG(prod_price) AS avg_price FROM Products`
`select AVG(prod_price) AS avg_price FROM Products WHERE vend_id = 'DLL01'`

>***警告*** `AVG`函数只能用于单个列,而且列名必须作为函数的参数给出,如果需要获得多个平均值函数,则需要使用多个`AVG`函数
> ***
>***说明***`AVG()`函数会忽略值为`NULL`的行.

**`COUNT()`函数**
`COUNT()`函数可以确定表中行的数目或者符合特定条件的行的数目,`COUNT()`函数有两种使用方法:
* 使用COUNT(*)对表中的数据进行计数,不论列表中包含的值是空值`NULL`还是非空值
* 使用`COUNT(colume)`对特定列具有值的行进行计数,忽略`NULL`值

  *eg.*
  `select COUNT(*) AS num_cust FROM Customers;`
  `select COUNT(cust_email) AS num_cust FROM Customers`

在上面两个例子中,第一个例子对所有的行计数,不管行中有什么值,计数值在`num_cust`中返回.而第二个例子中只对具有电子邮件地址的客户计数.

> *说明NULL值* 如果指定列名,则`COUNT()`函数会忽略指定列的值为空的行,但是`COUNT()`函数中用的是星号`(*)`,则不忽略.


**MAX()函数**
MAX()函数返回指定列中的最大值,MAX()要求指定列名,
*eg.*
`SELECT MAX(prod_price) AS max_price FROM Products`
>***提示***
对非数值型数据使用`MAX()`函数,虽然`MAX()`函数一般用来找出最大的数值或者日期,但许多(并非所有)DBMS允许将他用来返回任意列中的最大值,包括返回文本列中的最大值.在用于文本数据时,`MAX()`返回按该列排序的最后一行.
> ***
>***说明:NULL值***
>`MAX()`函数忽略值为`NULL`的行

**MIN()函数**
`MIN()`函数的功能和`MAX()`函数的功能刚好相反,他返回指定的最小的值.和`MAX()`函数一样,`MIN()`函数要求指定列名.
*eg.*
`SELECT MIN(prod_price) AS min_price FROM Products`
>***提示***
对于许多非数值行的数据使用`MIN()`函数,虽然`MIN()`函数一般用来查找出最小的数值或者日期,但是也和`MAX()`函数类似,也可以用来返回文本列表中的最小值,在用于文本数据的时候`MIN()`返回按该列排序的第一行.
>***
>***说明NULL值***
>`MIN()`函数会略值为`NULL`的行.

***SUM()函数***
`SUM()`函数用来返回指定列的和(总计).
`SELECT SUM(quantity) AS item_ordered FROM OrderItems WHERE order_num = 2005`
`SELECT SUM(item_price * quantity) AS total_price FROM OrderItems WHERE order_num = 20005`
>**分析**
>函数SUM(item_price * quantity) 返回订单中所有物品价格之和,WHERE字句同样保证只统计某个物品订单中的物品
> ***
>**提示:在多个列上计算**
>利用标准的算数操作符,所有聚集函数都可以用来执行多个列上的计算
> ***
>**说明**
>SUM函数忽略值为NULL的行

## 聚集不同值
以上五个聚集函数都可以如下使用:
* 对所有行执行计算,指定ALL参数或不指定参数(因为ALL是默认行为)
* 只包含不同的值,指定DISTINCT参数
> ***提示:ALL为默认***
>ALL参数不需要指定,因为他是默认行为.如果不指定DISTINCT,则假定为ALL
```
SELECT AVG(DISTINCT prod_price) AS avg_price FROM Products WHERE vend_id = 'DLL01';
```
在使用了DISTINCT关键值之后,在计算平均值的时候,就会排除相同的值,只使用不同的值来计算.
> **DISTINCT不能使用COUNT(*)**
> 如果指定列名,则DISTINCT只能用于COUNT(),DISTINXT不能用于COUNT(*).类似的,DISTINCT必须使用别名,不能用于计算或表达式
> **提示:将DISTINCT用于MIN()和MAX()**
> 虽然DISTINCT从技术上可用于MIN()和MAX(),但实际上这样做没有价值,一个列中最小值和最大值不管是否只考虑不同值,结果上都是一样的.

## 组合聚集函数
目前为止,所有的聚集函数都例子都只涉及单个函数,但是实际上,SELECT语句可以根据需要包含多个聚集函数,
`SELECT COUNT(*) AS num_item MIN(prod_price) AS price_min MAX(prod_price) AS price_max ,AVG(prod_price) AS price_avg FROM Products`
>**分析:**
>这里的单条SELECT语句执行了四个聚集计算,返回4个值
>**警告:取别名**
>在指定别名以包含某个聚集函数的结果时,不应该使用表中实际的列名,虽然这样做也算合法,但是许多SQL实现不支持,可能会产生模糊的错误信息

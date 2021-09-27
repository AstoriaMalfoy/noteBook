# 创建高级联结
## ==使用表别名==
在SQL中除了可以对列名和计算字段使用别名,还可以给表起别名,这样做有两个主要理由:
* 允许缩短SQL语句.
* 允许在一条SELECT表中多次使用相同的表
``` SQL
SELECT cust_name,cust_contact
    FROM Customers AS C ,Orders AS O , OrderItems AS OI
    WHERE c.cust_id = O.cust_id
        AND OI.order_num = O.order_num
        AND prod_id = 'RGAN01';
```
上看的SQL语句和之前的功能基本相同,但是改用了别名.

> 在Oracle中没有关键字AS,Oracle不支持AS关键字.要在Oracle中使用别名,可以不用AS,简单地指定列名即可`Customers C`

并且如果使用别名,表的别名是不反馈给客户端的.
## ==使用不同类型的联结==
在SQL中,一共有三种联结方式:自联结,自然联结和外联结

### ==自联结==
如前所述,使用表别名的原因是在一条SQL语句中不止一次的引用同一张表.
如果需要查询给与Jim Jones同一公司的所有顾客发送一封邮件,如果不使用别名可以使用如下的SQL:
``` SQL
SELECT cust_id,cust_name,cust_contact
    FROM Customers
    WHERE cust_name = (
            SELECT cust_name
            FROM Customers
            WHERE cust_contact = 'Jim Jones'
        );
```
如上是一种解决方案,使用了子查询.内部的SELECT做了一个简单的检索,返回jim jones工作公司的cust_name,该名字用户外部查询的WHERE字句,以检索出该公司所有的雇员.
实际上还可以使用联结来进行查询:
```SQL
SELECT c1.cust_id,c1.cust_name,c1.cust_contact
    FROM Customers AS c1,Customers AS c2
    WHERE c1.cust_name = c2.cust_name
        AND c2.cust_contact='Jim Jones';
```
此查询中,联结的两个表是相同的表,因此Customers在FROM字句中出现了两次,但是这是完全合法的.
> **提示:使用联结而不是使用子查询**
> 自联结通常作为外部语句,用来代替从相同表中检索数据使用的子查询语句.虽然最终得到的结果是完全相同的,但是许多DBMS处理联结远比处理子查询快速的多,所以应该两种方式都使用下,以确定那种方式性能更好.

### ==自然联结==
无论如何对表进行联结,应该至少都有一列是不止出现在一个表中(被联结的列).标准的联结返回的数据,相同的列甚至多次出现,自然联结排除多次出现的列,使得每个列只返回一次.
但是系统是不完成这项工作,需要由使用者自己完成,自然联结要求只能选择哪些唯一的列,一般通过对一个表使用通配符(SELECT *),而对其他表的列使用明确的子集来完成.

### ==外联结==
许多联结间给一个表中的行与另一个表中的行相联结,但有时候需要包含一些无法关联的行,比如一个另一个表中没有与之像符合联结关系的行.例如:
* 对每个顾客下的订单进行计数,包括那些至今尚未下单的顾客
* 列出所有产品以及订购数量,包括没有人订购的产品
* 计算平均销售规模,包括那些至今未下单的顾客.
  在上述列子中,联结包含了那些在相关表中没有关联行的行.这种联结称为外联结.
>**警告:语法差别**
>需要注意,用来创建外联结的语法在不同的SQL实现中可能少有不同
```SQL
SELECT C.cust_id  , O.order_num
    FROM Customers AS C  LEFT OUTER JOIN Orders O on C.cust_id = O.cust_id;
```
在这条SQL语句中使用了外联结,在使用`OUTER JOIN`语法时,必须要指定`RIGHT`或者`LEFT`关键字执行包括其所有行的表.
>`SQLite外联结`:SQLite支持`LEFR OUTER JOIN`,但是不支持`RIGHT OUTER JOIN`.幸好,其有一种更简单的方法.
> 总是有两种基本的外联结形式:左外联结和右外联结,他们之间的唯一差别是关联的表的顺序.换句话说,调整FROM或者WHERE字句顺序中表的顺序,左外联结可以转换为右外联结.因此,两种外联结可以互换使用,那个方便就使用那个.

还有一种外联结,就是全外联结.他检索两个表中的所有行并关联其中可以关联的行.左外联结或右外联结包含一个表的不关联的行不同,全外联结包含两个表中不关联的行(`FULL OUTER JOIN`).
>**警告:FULL OUTER JOIN的支持**
>Access,MariaDB,MYSQL,Open Office Base或者SQLite不支持`FULL OUTER JOIN`;

## ==使用具有聚集函数的联结==
使用聚集函数进行数据汇总的时候,联结表字段也同样可以使用.
```SQL
SELECT c.cust_id,COUNT(o.order_num) AS num_order
    FROM Customers AS C LEFT OUTER JOIN Orders O on C.cust_id = O.cust_id
    GROUP BY C.cust_id;
```
## ==使用联结和联结条件==
下面是联结及其使用要点:
* 注意所使用的联结的类型,一般我们使用内联结,但使用外联结也有效
* 关于确切使用联结的语法,应该查看具体的文档,看相应的DMBS支持何种语法
* 保证使用正确的联结条件,否则就返回不正确的数据.
* 应该总是提供联结条件,否则会得出笛卡尔积
* 在一个联结中可以包含多个表,甚至可以对每个联结采用不同的联结类型,虽然这种做法是合法的,一般也很有用,但是应该在一起测试他们之前分别测试每个联结,这样就使得排除故障更为简单.
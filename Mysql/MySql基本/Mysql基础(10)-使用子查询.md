# 子查询
# ==子查询==
> 查询 : 任何SQL语句都是查询,但是术语一般指SELECT语句

> 说明:如果使用MYSQL,应该知道在MYSQL中是支持的是从4.1开始的.MYSQL早期的版本并不支持自查询

# ==利用子查询进行过滤==
在给定的样例表中,如果需要查询出所有订购RGAN1的所有顾客,可以使用下面的自查询
``` SQL
SELECT `order_num` FROM OrderItems WHERE `prod_id`="RGAN01";

SELECT `cust_id` FROM `Orders` WHERE `order_num` in (
    SELECT `order_num` FROM OrderItems WHERE `prod_id`="RGAN01"
    );

SELECT * FROM `Customers` WHERE `cust_id` in (
    SELECT `cust_id` FROM `Orders` WHERE `order_num` in (
        SELECT `order_num` FROM OrderItems WHERE `prod_id`="RGAN01"
        )
    );
```
在SELECT语句中,自查询总是从内向外处理,处理最外侧的SELECT时,DBMS实际上进行了两部操作,首先执行查询``` SELECT `order_num` FROM OrderItems WHERE `prod_id`="RGAN01";```,此时查询结果返回两个订单号20007-20008,然后,这两个值以IN操作符要求的逗号分隔的格式传递给外部查询的WHERE字句,外部查询变为```“SELECT cust_id FROM orders WHERE order_num IN (20007,20008)”;```
>格式化SQL:
>包含自查询的SELECT语句难以阅读和调试,特别是在自查询较为复杂的时候,能够将子查询进行适当的缩进,能够极大的简化子查询的使用.

为了执行上述这个SELECT语句,DBMS必须要执行三条SQL语句,执行顺序一次从内到外,可见,在WHERE字句中使用自查询可以编写出许多功能很灵活的SQL语句,但是由于嵌套的子查询的数目没有限制,所以在使用的时候由于性能的限制,不能进行过多的嵌套.
> 作为子查询的SQL语句只能查询单个列,企图检索多个列将就报错.
> 子查询的性能:虽然这里的代码能够实现所需的结果,但是使用子查询实际上并不是检索这类数据的最有效的方法.

# ==作为计算字段使用子查询==
使用子查询的另一种方式是创建计算字段,在样例表中,如果需要显示Customsers表中每个顾客的订单总数,可以使用如下子查询来实现:
``` SQL
SELECT `cust_id`,(
        SELECT COUNT(*) FROM `Orders` WHERE Orders.cust_id = Customers.cust_id
    )
    AS orders FROM Customers ORDER BY cust_name;

```
这条语句对Customers表中每个顾客返回两条信息,`cust_id`和`orders`,不过orders是一个计算字段,是由圆括号中的子查询建立的,该子查询对检索出的顾每个执行一次,所以一共执行了5次.
>在子查询的SQL语句中,WHERE后面使用了全限定列名,这是因为当前是操作多个表,并且cust_id在两个表中都是存在的,此时就需要使用权限定列名,用来防止混淆,而聪明的做法是,只要在涉及多个表查询的时候,就应该使用权限定列名来代替普通的列名来方式混淆.

>同上一部分,这条语句同样不是解决这个问题最好的方法.


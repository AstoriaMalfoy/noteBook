# 连结表

## ==联结==

SQL最强大的功能就是在查询数据的时候执行联结(Join),连结表是利用SQL的SELECT能执行的最重要操作

### ==关系表==

在标准数据库表设计过程中,关系表为E-R图中的R表,用于存储实体间的对应关系,能够方便数据更简单,有效的处理.

> 可伸缩:能够适应不断增加的工作量而不失败,设计良好的数据库或应用程序称为可伸缩性好

### ==使用联结的原因==

> 将数据分解为多个表能够更加有效的存储,更方便的处理,并且可伸缩性更好.但是这是有代价的.如果数据存储在多个表中,在进行检索的时候就会复杂一些.这时候就需要使用联结才可以.

## ==创建联结==

创建联结的方式很简答,只需要执行所有需要联结的表以及他们的联结方式即可.

```SQL
SELECT vend_name,prod_name,prod_price
       FROM Vendors,Products
WHERE Vendors.vend_id = Products.vend_id;
```

> 在上述SQL中,SELECT字句后面选择的依旧是列名,不同的是FROM字句后面选择了两个表,这两个表就是这条SQL语句联结的来给那个表的名字,并且这两个表用WHERE字句进行联结.

> 在使用联结时,因为表的数量不唯一,可能会造成歧义,所以必须使用完全限定列名.如果没有使用全限定列名,大多数DBMS都会报错.

## ==WHERE字句==

在一条SELECT语句联结几个表的时候,相应的关系是在运行中构建的.在数据库表的定义中没有指示DBMS如果对表进行联结的内容,所以必须在SQL中指出.在联结两个表的时候,实际上要做的就是将第一个表的第一行和第二个表中的每一行进行配对,WHERE字句作为过滤条件,只包含哪些匹配的给定条件的行,如果没有WHERE字句,第一个表中的每一行将和第二个表中的每一行进行配对,而不管他们在逻辑上是否相关.

> 笛卡尔积:由于没有联结条件的表关系返回的结果为笛卡尔积,检索出的行的数目将是第一个表中的行数乘与第二个表中的行数.

> 在使用联结的时候需要保证有WHERE字句,否则DBMS返回的将会是笛卡尔积,同时,需要保证WHERE字句的正确性.

> 有时候返回笛卡尔积的联结也叫做叉联结

## ==内联结==

目前使用的所有联结都是等值联结,是基于两个表中的数据相同进行测试,这种联结也称为内联结,实际上可以使用稍微不同的语法,明确指定联结的类型,例如下面的SQL和之前的联结SQL作用完全相同.

```
SELECT vend_name,prod_name,prod_price FROM Vendors INNER JOIN Products P on Vendors.vend_id = P.vend_id;
```

> ANSI SQL 规范首选使用INNER JOIN语法,而使用WHERE是简单等值语法,而DBMS的确支持简单格式和标准格式,至于使用那个就看个人喜好

## ==联结多个表==

SQL并不限制一条SELECT语句中可以联结的表的数目,创建联结的关系也基本相同,首先列出所有的表,然后定义表之间的关系
例如:

```SQL
SELECT prod_name,vend_name,prod_price,quantity
    FROM OrderItems,Products,Vendors
    WHERE Products.vend_id = Vendors.vend_id
        AND OrderItems.prod_id = Products.prod_id
        AND order_num=20007;
```

> 考虑性能,DBMS在运行时关联指定每个表,以处理联结,但是这种处理可能非常浪费资源,因此需要注意,非必要的表不要联结,并且联结的表越多,性能下降的越厉害.

> 虽然在SQL中没有限制每个联结表的数目,但是实际上许多DBMS都有限制,在实际使用的时候还需查询相关DBMS文档从而了解详情

> 在实际应用的时候要执行一个操作很多时候有多种实现方式,很少有绝对正确或者绝对错误的方法,但是性能可能会受到操作类型,所使用的DBMS,表中的数据量,是否存在索引或者键等条件影响,因此需要不断尝试选择不同的机制,找出最合适的方法.

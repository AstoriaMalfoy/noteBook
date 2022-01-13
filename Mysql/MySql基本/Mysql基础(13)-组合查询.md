# 组合查询

## ==组合查询==

多数SQL查询只包含一个或多个表中的单条SELECT语句.但是,SQL也允许执行多个查询(多条SELECT语句),并将结果作为一个查询的结果集合返回,这些组合查询通常称为并(union)或者复合查询(compound query)
主要有两种情况是用组合查询:

* 在一个查询中中从不同的表中返回结构数据
* 对一个表执行多次查询,按照一个查询返回结果

> **组合查询和多个WHERE条件:** 多数情况下,组合相同表的两个查询所完成的工作与具有多个WHERE字句条件的一个查询所完成的工作相同.换句话说,任何具有多个WHERE字句的SELECT语句都可以作为一个组合查询,在下面可以看到这一点

## ==创建组合查询==

使用UNION操作符号可以组合数条SQL查询,利用UNION,可给出多条SELECT语句,并将他们的结果组合成一个结果集合

### ==使用UNION==

使用UNION的方式很简单,所要做的就是给出每条SQL,然后在各条语之间放入关键字UNION.

```SQL
SELECT cust_name,cust_contact,cust_email
FROM Customers
WHERE cust_state IN ('IL','IN','MI')
UNION
SELECT cust_name,cust_contact,cust_email
FROM Customers
WHERE cust_name='Fun4All';
```

上述语句由两条SELECT语句合并而来,其功能等价于如下:

```SQL
SELECT cust_name,cust_contact,cust_email
FROM Customers
WHERE cust_state IN ('IL','IN','MI') OR cust_name='Fun4All';
```

> **提示:UNION的使用:** 使用UNION组合SELECT语句的数目,SQL标准中并没有限制.但是,最好是参考一下具体的DBMS文档,了解他是否对UNION能组合的最大语句数目有限制.

> **警告:性能问题:** 多数好的DBMS使用内部查询优化程序,在处理各条SELECT语句前组合他们,理论上讲,这意味着从性能上看使用多条WHERE字句条还是UNION应该没有实际的差别.不过仅仅指的是理论上,实践中多数的查询优化程序并不能达到理想状态,所以最好同时测试下两种方法,在进行选择.

### ==UNION规则==

使用UNION时需要注意几条规则:

* UNION必须有两条或两条以上的SELECT语句组成,语句之间使用关键字UNION分隔.
* UNION中的每个查询必须包含相同的列、表达式、或者聚集函数,但是这些列不必按完全相同的顺序给出.
* 列数据类型必须兼容:类型不必完全相同,但是必须是DBMS可以隐含转换的类型.

### ==包含或取消重复的行==

在使用UNION的时候,其默认会去除重复的行,但是如果使用UNION ALL来进行联结的话,就会显示所有的行并且包括重复的行

> **提示:UNION与WHERE:** 在最开始说过,UNION极狐总是完成与多个WHERE条件相同的工作,UNION ALL作为UNION的一种形式,他完成WHERE完成不了的工作,如果实际应用中需要每个条件匹配的行全部出现,那么就必须使用UNION ALL 而不能使用WHERE

### ==对组合查询结果进行排序==

SELECT 语句的输出用ORDER BY字句排序.在UNION组合排序的时候,只能使用一条ORDER BY语句,他必须位于最后一条SELECT语句之后.对于结果集,不存在一部分使用一种方式排序,而另一部分使用其他的方式进行排序,因此不允许使用多条ORDER BY语句.

```SQL
SELECT cust_name,cust_contact,cust_email
FROM Customers
WHERE cust_state IN ('IL','IN','MI')
UNION
SELECT cust_name,cust_contact,cust_email
FROM Customers
WHERE cust_name='Fun4All'
ORDER BY cust_name;
```

> 虽然在SQL的最后一条语句中添加了ORDER BY 语句,但是实际上DBMS将就排序所有SELECT语句返回的所有结果

> **说明:其他类型的UNION**
> 在某些DMBS还支持另外两种UNION:EXCEPT(有时称为MINUS),可用来检索只在第一个中存在而在第二个表中不存在的行;而INTERSECT可以检索两个表中都存在的行,实际上,这些UNINO很少使用,因为相同的结果可以使用联结得到.

> **提示:操作多个表**
> 为了简单,上述的例子都是使用UNION来组合针对同一个表的多个查询.实际上,UNION在需要组合多个表的数据也是有用的,即使有列名不匹配的表,在这种情况下,也可以将UNION与别名组合,检索一个结果集.

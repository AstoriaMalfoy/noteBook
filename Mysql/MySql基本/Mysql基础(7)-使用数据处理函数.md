# ==使用数据处理函数==
几乎所有的DBMS都支持函数的使用，但是每一个DBMS都有自己特定的函数，与SQL语句不同，SQL函数是不可移植的，所以在使用的时候要做好取舍。

## 使用函数
大多数SQL都支持如下类型的函数
* 用于处理文本字符串的函数
* 用于在数值上进行算数计算的函数
* 用于处理日期和实践
* 返回DBMS正在使用的特殊信息的系统函数

## 文本处理函数
常用的文本处理函数,下表列出了一些常用的函数:
|函数|说明|
|:--|:--|
|LEFT()(或使用子字符串函数)|返回字符串左边的字符|
|LENGTH()(也使用DATALENGTH()或LEN())|返回字符串长度|
|LOWER()|将字符串转换为小写|
|LTRIM()|去掉字符串左边的空格|
|RTRIM()|去掉字符串右边的空格|
|RIGHT()|返回字符串右边的字符|
|SOUNDEX()|返回字符串的sounded值|
|UPPER()|将字符串转换为大写|

* `soundex()` 是一种语音算法,利用英文的读音计算相似值,通过由四个字符组成的代码来评估两个字符串的相似性.


## 日期和时间处理函数
因为一般的程序不使用日期和时间的存储格式,因此日期和时间的函数总是用来读取和统计这些值.由于这个原因,日期和时间函数在SQL中具有重要的作用.遗憾的是,日期和时间处理函数在各个DBMS中差异性很大,可移植性最差.

## 数值处理函数
数值处理函数仅仅用来处理数值函数.这些函数一般主要用于代数、三角或者几何运算,因此不不像字符串处理函数使用那么频繁.


|函数|说明|
|:--|:--|
|ABS()|返回一个数的绝对值|
|COS()|返回一个角度的余弦值|
|EXP()|返回一个数的指数值|
|PI()|返回圆周率|
|SIN()|返回一个角度的正弦值|
|SQRT()|返回一个数的平方根|
|TAN()|返回一个数的正切值|


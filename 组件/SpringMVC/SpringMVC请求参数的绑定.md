# SpringMVC请求参数的绑定

## 请求参数的绑定说明：

* 绑定机制
  
  * 表单提交的数据是Key-Value格式的，例如`uername = astoria`
  
  * SpringMVC的参数绑定过程是将表单提交的请求参数作为控制器中方法的参数进行绑定的。
  
  * 要求：提交表单的name和参数的名称是相同的

* 支持的数据类型
  
  * 基本数据类型和字符串数据类型
  
  * 实体类型（JavaBean）
  
  * 集合数据类型（List,Map）

## 基本数据类型和字符串类型

* 提交表单的name和参数的名称是相同的

* 区分大小写

## 实体类型（JavaBean）

* 提交表单的name和JavaBean中的属性名称需要保持一致

* 如果一个JavaBean中包含其他的引用类型，那么表单的name属性需要编写成 对象.属性，例如 address.name

## 提交中文

如果在解析参数的时候中文乱码就需要使用使用Filter过滤器将请求的编码设置为UTF-8，具体的设置方式是需要在web.xml文件中添加filter标签，配置类为`CharacterEncodingFilter`，并且需要设置初始化参数`encoding`为`UTF-8`

## 提交List和Map数据

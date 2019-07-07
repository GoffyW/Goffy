#### MyBatis总结

##### 配置文件与映射文件

##### MyBatisCRUD操作

##### ResultType与ResultMap区别

实体类与表中字段不一致时

1.  通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致，这样就可以表的字段名和实体类的属性名一一对应上了，这种方式是通过在sql语句中定义别名来解决字段名和属性名的映射关系的。
2. 通过<resultMap>来映射字段名和实体类属性名的一一对应关系。这种方式是使用MyBatis提供的解决方式来解决字段名和属性名的映射关系的

##### 动态SQL

​	if，choose (when, otherwise)，trim (where, set)，foreach

##### 一对一，一对多，多对多高级映射（association,collection）

1. 一对一关联，使用assacation标签
2. 一对多关联，使用collection标签。collection标签来解决一对多的关联查询，ofType属性指定集合中元素的对象类型         

##### 延迟加载

​	mybatis默认没有开启延迟加载，需要在MyBaits-Config.xml中setting配置

​	使用association、collection能实现表间关联，association、collection也具备延迟加载功能延迟加载：先从单表查询、需要时再从关联表去关联查询，大大提高数据库性能，因为查询单表要比关联查询多张表速度要快。

##### 一级缓存、二级缓存

1. 一级缓存默认开启，作用域一个sqlSession中，两个查询之后第二次查询会在缓存之中查找，提交查找效率，如果进行commit()（insert,update,delete）操作，则清空缓存。
2. 二级缓存默认开启，需要在xml文件中配置，其作用域为一个Mapper，即namespace，所以需要在对应的xml文件中进行配置。而且缓存可自定义存储源（Redis,EhCache,MemcCache）

##### 分页、批量

1. mybatis的分页功能，可以自己利用mysql代码实现，也可以利用mybatis分页插件,如pageHelper
2. 批量则依赖于SQL语句

##### 逆向工程
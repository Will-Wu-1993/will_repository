1. Mybatis的优点：
    基于SQL语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成影响，SQL写在XML里，接触SQL与程序的代码耦合，便于统一管理；提供XML标签，支持编写动态SQL语句，并可重用。
    与JDBC相比，减少了50%以上的代码量，消除了大量冗余的代码，不需要手动开关连接；
    很好的与跟中数据库兼容，（因为Mybatis使用JDBC来连接数据库，所以只要JDBC支持的数据库，Mybatis都支持）
2. Mybatis框架的缺点：
    SQL语句的编写工作量大，尤其当字段多、关联的表多时，对开发人员编写SQL语句的功底有一定的要求。
    SQL语句依赖数据库，导致数据库移植性差，不能随意更换数据库。
3. Mybatis和Hibernate有那些不同：
    Mybatis和hibernate不同，它不是一个完整的ORM框架，因为Mybatis需要程序员自己编写SQL语句。
    mybatis直接编写原生的SQL语句，可以严格控制SQL执行性能，灵活度高，非常适合对关系数据库模型要求不高的软件开发，因为这类软件需求变化频繁，一旦需求变化要迅速输出成果。但是灵活性的前提时mybatis无法做到数据库无关性，如果需要实现支持多重数据库的软件，则需要自定义，多套sql映射文件，工作量大。
    Hibernate对象/关系映射能力强，数据库无关性好，对于关系型模型要求高的软件，如果使用Hibernate开发可以节省很多代码，提高效率。
4.  **#**{}和${}的区别是什么？
    **#**{}是预编译符号，${}是字符串替换。
    mybatis在处理#{}时，会讲SQL中的#{}替换为？号，调用PreparedStatement方法来赋值；
    mybatis再处理${}时，就是把${}替换成变量值。
    使用#{}可以有效的防止SQL注入，提高系统安全性。
5. 通常一个xml映射文件，都会写一个Dao接口与之对应，请问，这个Dao接口的工作原理是什么？Dao接口的方法，参数不同时，方法能重载吗?
    Dao接口即Mapping接口。接口的全限名，就是映射文件中的namespace的值；接口的方法名，就是映射文件中mapper的Statement的id值；接口方法内的参数，就是传递给sql的参数。
    Mapper接口时没有实现类的，当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个MapperStatement。在Mybatis中，每一个`<selecy>`、`<insert>`、`<update>`、`<delete>`标签，都会被解析为一个MapperStatement对象。
    Mapper接口里的方法，是不能重载的，因为时使用全限名+方法名的保存和寻找策略。Mapper接口的工作原理时JDK动态代理，Mybatis运行时会使用JDK动态代理为Mapper接口生成动态代理对象proxy，代理对象会拦截接口的方法，转而执行MapperStatement所代表的SQL，然后将sql执行结果返回。
6. Mybatis时如何进行分页的？分页插件的原理时什么？
    Mybatis使用RowBounds对象进行分页，他是针对ResultSet结果集执行的内存分页，而非物理分页。可以在SQL内直接书写带有分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。
    分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截执行的SQL，然后重写SQL，根据dialect方言，添加对应的物理分页语句和物理分页参数。
7. Mybatis时如何将sql执行的结果封装为目标对象并返回的？都有那些映射形式？
    第一种是使用`<resultMap>`标签，逐一定义数据库列明和对象属性之间的映射关系。
    第二种是使用sql列的别名功能，将列的别名书写对象属性名。
    有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。
8. Mybatis动态SQL有什么用？执行原理？有哪些动态SQL？
    Mybatis动态SQL可以在xml映射文件内，以标签的形式编写SQL，执行原理是根据表达式的值完成逻辑判断并动态拼接SQL的功能。
    Mybatis提供了9种动态SQL标签：trim，where，set，foreach，if，choose，when，otherwise，bind。
9. XML映射文件中，除了常见的标签之外，还有那些标签？
    `<resultMap>`、`<parameterMap>`、`<sql>`、`<include>`、`<selectKey>`，加上动态SQL的9个标签，其中为SQL片段标签，通过`<include>`标签引入SQL片段，`<selectKey>`为不支持自增的主键生成策略标签。
10. Mybatis的XML映射文件中，不同的XML映射文件，id是由可以重复？
    不同的XML映射文件，如果配置了namespace，那么id是可以重复的，如果没有配置namespace，那么id是不能重复的。
    原因就是namespace+id是作为`Map<String,Mapper<MapperStatement>>`的key使用的，如果没有namespace，就剩下id，那么id重复会导致数据互相覆盖。有了namespace，自然id就可以重复，namespace不同，namespace+id自然就不同。

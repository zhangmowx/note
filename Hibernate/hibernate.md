# Hibernate 面试题

## 1.Hibernate工作原理及为什么要用？
1. 通过Configuration().configure();读取并解析hibernate.cfg.xml配置文件
2. 由hibernate.cfg.xml中的<mappingresource="com/xx/User.hbm.xml"/>读取并解析映射信息
3. 通过config.buildSessionFactory();//创建SessionFactory
4. sessionFactory.openSession();//打开Sesssion
5. session.beginTransaction();//创建事务Transation
6. persistent operate持久化操作 //一般指Save这个方法
7. session.getTransaction().commit();//提交事务
8. 关闭Session
9. 关闭SesstionFactory

## 2.为什么要用Hibernate
1. 对JDBC访问数据库的代码做了封装，大大简化了数据访问层繁琐的重复性代码。
2. Hibernate是一个基于JDBC的主流持久化框架，是一个优秀的ORM实现。他很大程度的简化DAO层的编码工作
3. hibernate使用Java反射机制，而不是字节码增强程序来实现透明性。
4. hibernate的性能非常好，因为它是个轻量级框架。映射的灵活性很出色。它支持各种关系数据库，从一对一到多对多的各种复杂关系。

## 3.Hibernate是如何延迟加载(懒加载)?
通过设置属性lazy进行设置是否需要懒加载
当Hibernate在查询数据的时候，数据并没有存在与内存中，当程序真正对数据的操作时，对象才存在与内存中，就实现了延迟加载，他节省了服务器的内存开销，
从而提高了服务器的性能。

## 4.Hibernate中怎样实现类之间的关系?(如：一对多、多对多的关系)
类与类之间的关系主要体现在表与表之间的关系进行操作，它们都是对对象进行操作，我们程序中把所有的表与类都映射在一起，
它们通过配置文件中的many-to-one、one-to-many、many-to-many，来实现类之间的关系。

## 5.hibernate的三种状态之间如何转换
 ### Hibernate中对象的状态：
  1. 瞬时状态 ：没有持久化标识OID，没有被纳入Session对象的管理
  2. 持久化状态：有持久化标识OID，已经被纳入Session对象的管理
  3. 游离状态：有持久化标识OID，但没有被Session对象管理
 ### 装换方式  
 ```
    使用new关键字构建对象，该对象的状态是瞬时状态。
    1. 瞬时状态转为持久状态
    使用Session对象的save()或saveOrUpdate()方法保存对象后，该对象的状态由瞬时状态转换为持久状态。
    使用Session对象的get()或load()方法获取对象，该对象的状态是持久状态。
    2. 持久状态转为瞬时状态
    执行Session对象的delete()方法后，对象由原来的持久状态变为瞬时状态，因为此时该对象没有与任何的数据库数据关联。
    3. 持久状态转为游离状态
    执行了Session对象的evict()--清除单个对象、clear()--清除所有对象或close()方法，对象由原来的持久状态转为游离状态。
    4. 游离状态转为持久状态
    重新获取Session对象，执行Session对象的update()或saveOrUpdate()方法，对象由游离状态转为持久状态，该对象再次与Session对象相关联。
    5. 游离状态转为瞬时状态
    执行Session对象的delete()方法，对象由游离状态转为瞬时状态。
    处于瞬时状态或游离状态的对象不再被其他对象引用时，会被Java虚拟机按照垃圾回收机制处理。
```

## 6.比较hibernate的三种检索策略优缺点
 ### 立即检索：
优点： 对应用程序完全透明，不管对象处于持久化状态，还是游离状态，应用程序都可以方便的从一个对象导航到与它关联的对象；  
缺点： 1.select语句太多；2.可能会加载应用程序不需要访问的对象白白浪费许多内存空间；  
立即检索:lazy=false；  

### 延迟检索：
优点： 由应用程序决定需要加载哪些对象，可以避免可执行多余的select语句，以及避免加载应用程序不需要访问的对象。因此能提高检索性能，并且能节省内存空间；
缺点： 应用程序如果希望访问游离状态代理类实例，必须保证他在持久化状态时已经被初始化；
延迟加载：lazy=true；

### 迫切左外连接检索：
优点： 1对应用程序完全透明，不管对象处于持久化状态，还是游离状态，应用程序都可以方便地冲一个对象导航到与它关联的对象。2使用了外连接，select语句数目少；
缺点： 1 可能会加载应用程序不需要访问的对象，白白浪费许多内存空间；2复杂的数据库表连接也会影响检索性能；
预先抓取： fetch=“join”；

## 7.Hibernate检查策略
 ### 1. 类级别检索策略
   #### 立即检索  get
    session.get(xxx) 直接发送SQL，到数据库查询，返回真实对象
   #### 延迟检索  load
    session.load(xxx) 不发送sql，返回一个代理对象，当需要使用该对象时，才发送sql。
    可以通过在配置文件<class>标签中配置lazy="false"属性，设置不延迟检索
 ### 2. 关联级别检索策略
 ```
 在<set>标签上或者在<many-to-one>上设置两个属性值来控制其检索策略，  fetch、lazy
　fetch：代表检索时的语句的方式，比如左外连接等。
   fetch：join、select 、subselect　　
  lazy：是否延迟加载。
   lazy：true、false、extra
   
   一对多或多对多时
   <set fetch=""，lazy="">
   fetch = join时，采取迫切左外连接查询　　
       lazy不管取什么值都失效，就是取默认值为false。
   fetch=select时，生成多条简单sql查询语句
       lazy=false：立即检索
       lazy=true：延迟检索
       lazy=extra：加强延迟检索，非常懒惰，比延迟检索更加延迟
   fetch=subselect时，生成子查询
       lazy=false:立即检索
       lazy=true；延迟检索
       lazy=extra：加强延迟检索，非常懒惰，比延迟检索更加延迟
   多对一或一对一时
  fetch可以取值为：join，select
  lazy：false，proxy，no-proxy
  当fetch=join，lazy会失效，生成的sql是迫切左外连接
  如果我们使用query时，hql是我们自己指定的，那么fetch=join是无效的，不会生成迫切左外连接，这时lazy重新启用
  当fetch=select，lazy不失效，生成简单sql语句，
  lazy=false：立即检索
  lazy=proxy：这时关联对象采用什么样的检索策略取决于关联对象的类级别检索策略.就是说参考<class>上的lazy的值
```
 ### 3. 批量检索
  解决n+1的问题，方案有2中
  1. 一的一方<set>中设置batch-size='x'
  2. 多的一方<class>中设置batch-size='x'
 
 ## 8. get和load区别
 ### 1. 返回方式
load方式检索不到的话会抛出org.hibernate.ObjectNotFoundException异常；
get方法检索不到的话会返回null；
 ### 2. 检索执行机制
get方法先到一级缓存，然后二级，最后db查找。  
load方法首先一级缓存中是否有缓存，如果有则直接返回，如果没有则去查找二级缓存，如果有则返回，如果没有则判断是否是lazy，若不是lazy，直接访问数据  库检索，查到记录返回（并且同时在二级缓存中存放查到的数据方便下次使用，若再下次使用时在二级缓存命中，就是查到数据，则有可能将数据放到一级缓存中。），查不到抛出异常。 若是lazy，则返回代理对象，而不到数据库中查找，除非使用此对象时，才到数据库中查找。

## 9. hibernate之saveorupdate()、save()、update()都有什么区别
saveorupdate()如果传入的对象在数据库中有就做update操作，如果没有就做save操作。  
save()在数据库中生成一条记录，如果数据库中有，会报错说有重复的记录。  
update()就是更新数据库中的记录  


https://wenku.baidu.com/view/b268a128af45b307e871970f.html?sxts=1563933439062

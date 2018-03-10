# spring源码分析



##### 1、IOC控制反转

- ioc容器（servlet）,存储javaBean
- web容器存储servlet
- 依赖注入（DI），对象之间的依赖关系，依赖链
  - setter方法注入

  - 构造器注入

  - 强制注入

    ​

##### *2、AOP 面向切面编程*

- 核心思想是解耦，先要把一个整体，给拆了，分别开发，等到发布的时候，再组装到一起运行

- 面向切面就是面向规则编程，只要有规则，就是可以使用切面

- 应用情景,如Authentication权限,Logging日志,Transactions Manager事务,lazy loaning懒加载,

  Context Process上下文处理,error handler错误跟踪(异常捕获机制),cache缓存处理

  ​

##### *3、常用设计模式*

- 动态代理模式

  - 原理：1、拿到被代理对象的引用，然后获取它的接口

    2、JDK代理重新生成一个类，同时实现我们给的代理对象所实现的接口

    3、把被代理对象的引用也拿到了

    4、重新动态生成一个class字节码

    5、然后编译

  - 满足代理模式应用场景的三个必要条件：1、两个角色：执行者、被代理对象

    2、注重过程，必须要做，被代理对象没时间做或者不想做，不专业

    3、执行者必须拿到被代理对象的个人资料（执行者持有被代理对象的引用）

  - 动态代理的底层概括：字节码重组

  - jdk动态代理与cglib动态代理。前者是通过接口来进行强制转换的，生成后的代理对象，

    ，可以强制转换为接口。后者是通过生成一个被代理对象的子类，然后重写父类的方法，

    生成以后的对象，可以强制转换为代理对象（也就是自已写的类），子类引用赋值给父类

- 工厂模式

  - 简单工厂、工厂方法、抽象工厂。特点，隐藏复杂的逻辑处理过程，只关心处理结果。
  - 设计模式的经典之处，就在于，解决了编写代码的人和调用代码的人双方的痛处，解放双手

- 单例模式

  - 穷举法：配置文件、直接上级领导

  - 特点：1、保证从系统启动到系统终止，全过程只会产生一个实例。

    2、当我们在应用中遇到功能性冲突的时候，需要使用单例模式。

- 委托模式

  - 特点：1、类似于中介的功能（委托机制）2、持有被委托人的引用 3、不关心过程，只关心结果。比如项目经理（委托人）和普通员工（受托人）之间的关系，干活是你的，功劳是我的。
  - 结果有多样性。工厂方法返回的是bean

- 策略模式

  特点：结果一致，关心实现过程。

- 原型模式

  - 实现cloneable接口，不走构造方法。字节码复制，浅拷贝（八种基本类型+String）
  - 特点：过程相同，但是结果不一样。2、数据内容完全一样，但实例不同

- 模板模式

  特点：执行流程一样，但中间有些步骤不同。

  ​

##### 4、springMVC的发展历程

​	java一开始有三种应用形式J2ME(mobile)、J2SE(Standard)自已开发出来自己玩（Swing开发，与C#相比多了很多配置,C#占有大量windows用户）、J2EE(Enterprise,只需在J2SE上加一个URL''统一资源定位符")，其中servlet就是其实现，servlet是server Applet,一般来说，加上servlet-api.jar的J2SE就是J2EE。容器就是用来装东西的，装servlet,servlet有doGet、doPost。

​	后来发现，随着业务变得复杂，实现太多servlet很难管理，所以才有mvc框架的出现，比如structs(实现一个自已的容器,把用户请求的url映射成某一个类(普通类),比如web/user!getList.do),你只要配置一个Filter,用来拦截所有url请求.

​	后来spring才慢慢发展自已的mvc框架,目的是不用反复的修改web.xml, 把每一个URL变成一个类的某个方法,传参变为一个自动ORM(变化开发)



##### 5、spring的IOC和DI源码分析

- BeanFactory是spring源码的入口，一切来自于BeanFactory

- IOC容器的初始化包括BeanDefinition的Resource定位、载入和注册三个基本过程。

- 在spring框架里，只要是以do开头的方法，都是具体干法的方法

- BeanDefinition来保存xml信息

- IOC容器就是一个map

- 资源定位（配置文件）、载入（读取配置文件）、注册（把加载后的配置文件解释成BeanDefinition），原则：宁可前面的过程复杂，最终要保证数据的有效性

- BeadFactory是生产Bean的工厂,而FactoryBead是Spring的Factory new出来的Bean

- DI(依赖注入),代码入口BeadFatory#getBean。

- spring创建出来的Bean一般是用代理模式生成的（cglib），除了用原型模式之外。好处：拥有这个代理类的控制权，提高了灵活度（做切面）

- 真正的IOC容器是factoryBeanObjectCache(ConcurrentHashMap<String,Object(Beandefinition)>)

- IOC组成体系结构中，有一 个重要的东西是BeanFactory，而且它被另两个重要的类继承，ListAbleBeanFactory和HierarchicalBeanFactory。另外还有一个类ClassPathXmlApplicantionContext,IOC的所有功能基本都可以在这个类里找到。

- 依赖注入。第一步，读取BeanDefinition中信息，获取其依赖关系；第二步，实例化（代理对象，对应create BeanInstance）；第三步，注入(对应populateBean)：设值

- spring最核心的两个jar包，spring-bean.jar、spring-context.jar。spring-bean.jar定义了规范 ，而spring-context.jar 工厂的实现、DI的实现。另外spring-core.jar是最顶层的jar，所有的项目都要依赖的。

- do开头的方法是具体干活的方法

- Spring 的单例是用Map来现实的,除非手动声明scope,prototype,每次get一次,就new一个

- IOC判断，如果被代理的类实现了一个接口，那么默认用JDK代理，如果被代理的对象，没有实现任何接口，那就默认用CGlib

  ​

##### 6、spring的AOP

- spring-aop是spring-aspects的上层建筑，而且它是从IOC中取得代理以后的对象，对每个方法进行重写，还加入一些切面调用所需要的东西。

- 一个切面，就代表N个Bean的一个集合，这N个Bean它们都拥有共同点,所以它们组成一个切面

- 通知(advice),利用了IOC的后置处理（方法拦截器，这是在AOP里面独有的）

- 切入点，就是在切面中，某一个具体Bean中的某一个具体方法.还可以是进入切面内部的一个入口(主动调用方法)

- 分析源码的第一步，找到入口，也是从factory入手的，找到一个getBean的方法，这个方法就是用来获取一个代理以的bean。这里就跟IOC体系的getBean有异曲同工

- AOP有启动的顺序在IOC容器启动（定位、载入、注册、初始化、注入）之后，AOP不用执行IOC创建对象的操作了

- IOC已经是代理类了，那么AOP中，对代理类的二次操作是深操作， 它们的每一个动作都要被管控

- AOP的整条链路的理解。1、加载配置信息，解析成AOPConfig。2、交给AopProxyFactory,调用creteAopProxy的方法 3、JdkDynamicAopProxy调用AdvisedSupport的getInterceptorsAndDynamicInterceptionAdvice方法 4、递归执行拦截器方法proceed()

- IOC容器就是一个ConcurrentHashMap,而Aop的MethodInterceptor容器是List

  ​

##### 7、spring的jdbc

1. SpringJdbc本身就是基于模板模式来开发的，JDBCTemplate

2. orm就是把resultset映射成简单的java对象
  - Hibenate优点：1、API丰富，可以实现无SQL操作（HQL）,为了兼容所有数据库（都会先解释为HQL）再由HQL翻译成SQL(当然，有支持直接执行SQL的API，为了考虑用户需要复杂性)，对所有数据库方言都支持得非常不错。2、ORM全自动化

  - Mybatis优点：1、轻量级，性能好。2、SQL和java代码分离（sqlMap,把每一条SQL语句起一名字，作为Map的key保存）

  - 自已要的框架（择其善者而从之，开发出一个合适自己的框架）：1、性能要好，是啥就是啥，不经过二次处理，2、单表操作实现NOSQL。3、ORM零配置实现自动化（利用反射机制，把字段和属性对应上，然后，自动实例化返回结果）。

  - 原则：约定优于配置（保证代码健壮性）

    - Dao原则：一个Dao只操作一张表
    - 约定：修改和删除的是根据主键来操作的
    - 约定：尽量使用单表操作，如果实在要多表操作，可以先把数据查出来放到内存，然后内存中进行计算
    - 约定：支持读写分离
    - 约定：支持分库分表
    - 约定：ORM支持的类型操作原则上只认Java八大基本数据类型+String(降低复杂度)

3. orm编写的过程

   1、制定QueryRule规范，定义了很多查询规则的常量，定义了查询规则保存到方法。

   2、提供一个抽象Dao类给用户去实现（基于单表操作，传两个泛型值，一个是主键，另一个实体类）。

   3、把实体的配置信息解析成一个EntityOpertion对象，同时实现了ORM的自动化过程

   4、在抽象的Dao调用查询方法，把QueryRule作为参数传入 。

   作为一个框架来说，要给用户操作提供一个方便的窗口，就相当于spring这么强大的一个框架，对于用户使用来说，就是一个方法ClassPathXmlApplicantion app;  app.getBean() 搞定所有的操作。所以应该提供一个QueryRule搞定所有的查询操作，这就是所有框架的创造者需要考虑的，最经典之处

   5、再将传入的QueryRule交给QueryRuleSqlBuilder构建出一个sql语句（被拆分了SQL）

   6、拼接CRUD SQL语句

   7、交给JdbcTemplate执行

   8、调用EntityOperation的ORM过程

   9、返回结果，对于用户来说，都是单表操作，而且规定了泛型，所以不需要做任何强制转换












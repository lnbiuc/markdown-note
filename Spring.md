# Spring创建对象

在Spring配置文件中加入<bean>标签

- `<bean id="user" class="com.beyondhoriozn.spring.User"/>`

调用对象

- 加载Spring配置文件

```java
ApplicationContext context = new ClassPathXmlApplicationContext("配置文件名");
```

- 获取已加载类中的对象

```java
User user = context.getBean("user", User.class);
```

- 调用类中的方法

```java
user.print();
```

# IOC

## ioc接口实现方式

### BeanFactory

ioc容器的基本实现，加载配置文件时不会创建对象，调用时才会创建

### ApplicationContext

BeanFactory的子接口，加载配置文件时会创建配置文件中的对象

### ApplicationContext实现类

- ClassPathXmlApplicationContex相对路径，指向xml文件地址
- FileSystemXmlApplactionContext绝对路径，指向sml文件地址

## ioc操作Bean管理(完全XML方式)

### 通过Spring创建对象

- 基于xml配置文件
    - `<bean id="user" class="com.beyondhoriozn.spring.User"/>`
    - class类全路径
    - 创建对象时，默认执行无参构造方法

- 通过Spring注入属性（赋值）
    - 通过set方法注入属性

```xml
<bean id="personal" class="com.beyondhoriozn.spring.Personal">
    <property name="name" value="Bot_Steven"></property>
    <property name="age" value="10"></property>
    <property name="sex" value="1"></property>
</bean>
```

- 通过有参构造注入属性

```xml
<bean id="student" class="com.beyondhoriozn.spring.Student">
    <constructor-arg name="name" value="Bot_Alex"></constructor-arg>
    <constructor-arg name="age" value="10"></constructor-arg>
</bean>
```

- 其他数据类型注入
    - null

```xml
<bean id="personal" class="com.beyondhoriozn.spring.Personal">
    <property name="sex">
        <null></null>
    </property>
</bean>
```

- 特殊符号

```xml
<property name="name">
    <value><![CDATA[<<o>>]]></value>
</property>
```

- 对象

    - 嵌套（外部bean）(级联赋值)
```xml
<bean id="userService" class="com.beyondhoriozn.spring.service.UserService">
    <property name="userDao" ref="userDao"></property>
</bean>
<bean id="userDao" class="com.beyondhoriozn.spring.dao.UserDaoImpl"></bean>
```

- 级联赋值另一种写法（需要get方法）
```xml
<bean id="employee" class="com.beyondhoriozn.spring.bean.Employee">
    <property name="employeeName" value="SecondEmployee"></property>
    <property name="boss" ref="boss"></property>
    <property name="boss.bossName" value="SecondBOSS"></property>
</bean>
<bean name="boss" class="com.beyondhoriozn.spring.bean.Boss"></bean>
```

- 内部bean  
```xml
<bean id="employee" class="com.beyondhoriozn.spring.bean.Employee">
    <property name="employeeName" value="FirstEmployee"></property>
    <property name="boss">
        <bean id="boss" class="com.beyondhoriozn.spring.bean.Boss">
            <property name="bossName" value="This is BOSS!"></property>
        </bean>
    </property>
</bean>
```

### 集合、数组、list、map、set

```xml
<bean id="collectionType" class="com.beyondhoriozn.spring.collection.CollectionType">
	<property name="arrays">
		<array>
			<value>array1</value>
			<value>array2</value>
		</array>
	</property>
	<property name="list">
		<list>
			<value>list1</value>
			<value>list2</value>
		</list>
	</property>
	<property name="map">
		<map>
			<entry key="1" value="value1"></entry>
			<entry key="2" value="value2"></entry>
		</map>
	</property>
	<property name="set">
		<set>
			<value>set1</value>
			<value>set2</value>
		</set>
	</property>
</bean>
```

### 对象数组List

```xml
<bean id="objectList" class="com.beyondhoriozn.spring.collection.ObjectList">
	<property name="objectList">
		<list>
			<ref bean="eachObject1"></ref>
			<ref bean="eachObject2"></ref>
		</list>
	</property>
</bean>
<bean id="eachObject1" class="com.beyondhoriozn.spring.collection.EachObject">
	<property name="objectName" value="object1"></property>
	<property name="objectCount" value="1"></property>
</bean>
<bean id="eachObject2" class="com.beyondhoriozn.spring.collection.EachObject">
	<property name="objectName" value="object2"></property>
	<property name="objectCount" value="2"></property>
</bean>
```

## ioc操作Bean管理(XML+Java方式)

bean中定义类型可以与返回类型不一样

### 例子

```java
public class UnSameBean implements FactoryBean<EachObject> {
    private UnSameBean unSameBeanName;
    
    @Override
    public EachObject getObject() throws Exception {
        EachObject eachObject = new EachObject();
        eachObject.setObjectName("FactoryBeanObjectName");
        eachObject.setObjectCount(1);
        return eachObject;
    }
}
```

xml配置

```xml
<bean id="unSameBean" class="com.beyondhoriozn.spring.factorybean.UnSameBean"></bean>
```

测试

```java
public void appUnSameBean() {
    Object object;
    ApplicationContext context = new ClassPathXmlApplicationContext("beanTest.xml");
    EachObject eachObject = context.getBean("unSameBean", EachObject.class);
    System.out.println(eachObject);
}
```

## bean多实例和单实例

`<bean scope="singleleton">`单实例,加载spring配置文件时创建单实例对象

`<bean scope="prototype">`多实例，调用getBean方法时创建多实例对象

### bean作用域

`<bean scope="request">`创建时对象后放到request域中

`<bean scope="session">`创建对象后放到session域中

### bean生命周期

1. 通过构造器创建bean实例（调用无参构造方法）
2. 为bean的属性设置值和对其他bean引用（set方法）
3. 调用bean的初始化方法（需要进行配置的初始化方法）
4. bean具体使用
5. 容器关闭时bean被销毁

### xml自动装配

```xml
<bean id="worker" class="com.beyondhoriozn.spring.autowire.Worker" autowire="byName"></bean>
<bean id="payment" class="com.beyondhoriozn.spring.autowire.Payment"></bean>
```

### 使用示例

#### xml+druit创建数据库连接池

```xml
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/dbName"></property>
    <property name="username" value="root"></property>
    <property name="password" value="root"></property>
</bean>
```

#### xml+druit+jdbc.propertiesl创建数据库连接池

jdbc.properties(放在src目录下)

```properties
prop.driverClass=com.mysql.jdbc.Driver
prop.url=jdbc:mysql://localhost:3306/dbName
prop.userName=root
prop.password=root
```

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${prop.driverClass}"/>
    <property name="url" value="${prop.url}"/>
    <property name="username" value="${prop.username}"/>
    <property name="password" value="${prop.password}"/>
</bean>
```

## ioc操作bean管理（基于XML+注解方式）

>  扫描该目录下所有的注解

```xml
<context:component-scan base-package="com.beyondhoriozn.spring.note"></context:component-scan>
```

```java
@Component
public class Peaceful {
    public void out() {
        System.out.println("successful!!--");
    }
}
```

>@Component
>
>@Service
>
>@Controller
>
>@Repository

仅扫描该目录下带@Service类型的注解

```xml
<context:component-scan base-package="com.beyondhoriozn.spring" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Service"/>
</context:component-scan>
```

> 基于注解方式属性注入

除了接口，所有类都需要使用注解的方式先创建对象

在需要被注入的地方写@autowired

```java
public interface Father {
    /**
     * out
     */
    public void out();
}
```

```java
@Service

public class FatherImpl implements Father {
    
    
    @Override
    public void out() {
        System.out.println("AutoWired Successful");
    }
}
```

```java
@Service
public class Son {
    
    @Autowired
    //根据类型注入，仅单例
    public Father father;
    
    public void sonOut() {
        System.out.println("Son Out");
        father.out();
    }
}
```

- 如果该接口有多个实现类，需要使用`@Qualifier(value = "matherImpl")`选定一个类进行注入

- 或者使用`@Resource(name = "matherImpl")`注入

- 使用`@Value(value = "valueOfString")`普通类型注入

## ioc操作bean管理(完全使用注解方式)

新建Config类

```java
@Configuration
@ComponentScan(basePackages = {"com.beyondhoriozn.spring.autowired"})
public class SpringConfig {
}
```

获取对象

```java
@Test
public void appConfig() {
    ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
    Son son = context.getBean("son", Son.class);
    son.sonOut();
}
```

# AOP

用于在不修改源文件的情况下，增强某一部分方法

## 基于注解方式

- 被增强的类

```java
@Component
public class Plus {
    public void addTwo() {
        System.out.println("Plus AddTwo Execution");
    }
}
```

- 写增强的具体内容

```java
@Component
@Aspect
public class PlusProxy {
            
    @Before(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")
    public void before() {
        System.out.println("前置通知");
    }    
}
```

- 给增强内容的类加上注解@Aspect使其创建代理对象

    - `@Aspect`

- 在Spring配置文件中开启@Aspect注解扫描

```xml
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

- 使用配置文件（必须）开启组件扫描

```xml
<context:component-scan base-package="com.beyondhoriozn.spring.aop"></context:component-scan>
```

- 使用注解创建两个类的对象，测试

- `@Component`

### 前置通知

在增强内容的类中使用`切入点表达式`

`@Before(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")`标记这个方法是前置通知的方法

```java
@Component
@Aspect
public class PlusProxy {
    
    @Before(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")
    public void before() {
        System.out.println("前置通知");
    }
    
}
```

### 后置通知

```java
@AfterReturning(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")
public void afterReturning() {
    System.out.println("后置通知");
}
```

### 异常通知

```java
@AfterThrowing(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")

public void afterThrowing() {
    System.out.println("异常通知");
}
```

### 环绕通知

```java
@Around(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")
public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
    System.out.println("环绕通知-1-");
    proceedingJoinPoint.proceed();
    System.out.println("环绕通知-2-");
```

### 最终通知

```java
@After(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")
public void after() {
    System.out.println("最终通知");
}
```

### 执行顺序

无异常时

> 环绕通知-1-
> 前置通知
> Plus AddTwo Execution
> 环绕通知-2-
> 最终通知
> 后置通知

有异常时

>环绕通知-1-
>前置通知
>最终通知
>异常通知
>
>java.lang.ArithmeticException: / by zero

## 抽取公共切入点

```java
@Pointcut(value = "execution(* com.beyondhoriozn.spring.aop.Plus.addTwo(..))")
public void publicPoint() {

}
```

### 调用

```java
@Before(value = "publicPoint()")
public void before() {
    System.out.println("前置通知");
}
```

## 多个增强类增强同一个方法，设置优先级

增强类前

```java
@Order(value = 0)
public class PlusProxy {
    
}
```

```java
@Order(value = 1)
public class PlusThreeProxy {
    
}
```

value值越小优先级越高

## 切点的申明规则

```java
execution（modifiers-pattern? ret-type-pattern declaring-type-pattern? name-pattern（param-pattern） throws-pattern?）
```

- ret-type-pattern：返回类型模式，决定方法返回类型必须依次匹配一个连接点。
- declaring-type-pattern：只匹配相同返回类型的方法
- name-pattern：匹配方法名
- param-pattern：匹配参数

# JdbcTemplate完整过程

- 创建service dao daoimpl类并注入对象

- 创建数据库配置文件
- spring配置文件中引入数据库配置文件
- spring配置文件中创建数据库连接池
- 创建jdbcTemplate并注入数据库连接池
- 开启组件扫描
- 将dao自动装配到service
- 将jdbcTemplate自动装配到daoImpl

```properties
prop.driverClass=com.mysql.jdbc.Driver
prop.url=jdbc:mysql://150.158.16.245:3306/springsql
prop.userName=violet
prop.password=tmYFY4KetFwRBwnw
```

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>

<context:component-scan base-package="com.horizon"/>

<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
    <property name="driverClassName" value="${prop.driverClass}"/>
    <property name="url" value="${prop.url}"/>
    <property name="username" value="${prop.userName}"/>
    <property name="password" value="${prop.password}"/>
</bean>

<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

```java
@Service
public class UserService {
    @Autowired
    private UserDao userDao;
}
```

```java
@Component

public interface UserDao {
}
```

```java
@Repository
public class UserDaoImpl implements UserDao{
    
    @Autowired
    private JdbcTemplate jdbcTemplate;
}
```

# Spring业务操作过程

```java
@Repository

public interface UserDao {
    /**
     * 添加记录
     */
    void upDate(UserEntity user);
}
```

```java
@Repository
public class UserDaoImpl implements UserDao{
    
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    
    
    @Override
    public void upDate(UserEntity user) {
    
        String sql = "insert into user values(?,?,?)";
    
        Object[] args = {user.getUserId(),user.getUserName(),user.getPassWord()};
        int upData = jdbcTemplate.update(sql, args);
        System.out.println(upData);
    }
}
```

```java
public class UserEntity {
    private int userId;
    private String userName;
    private String passWord;
    
    

    
    public int getUserId() {
        return userId;
    }
    
    public void setUserId(int userId) {
        this.userId = userId;
    }
    
    public String getUserName() {
        return userName;
    }
    
    public void setUserName(String userName) {
        this.userName = userName;
    }
    
    public String getPassWord() {
        return passWord;
    }
    
    public UserEntity(int userId, String userName, String passWord) {
        this.userId = userId;
        this.userName = userName;
        this.passWord = passWord;
    }
    
    public void setPassWord(String passWord) {
        this.passWord = passWord;
    }
}
```

```java
@Service
public class UserService {
    @Autowired
    private UserDao userDao;
    public void upData(UserEntity user){
        userDao.upDate(user);
    }
}
```

```java
public class SpringTest {
    
    @Test
    public void appUserService(){
        ApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");
        UserService userService = context.getBean("userService", UserService.class);
    
        UserEntity user = new UserEntity(4,"test3","test3");
        userService.upData(user);
    }
}
```

## 其他业务逻辑

> 查询返回对象使用

`jdbcTemplate.queryForObject(Strin sql, RowMapper<>() rowMapper, args)`方法

具体写法

```java
jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<UserEntity>(UserEntity.class), user.getUserName())
    //返回对象类型
```

RowMapper接口，针对不同的返回类型进行封装

> 批量操作

```java
public void fillUser(List<Object[]> userList) {
    userDao.addList(userList);
}
```

```java
public void addList(List<Object[]> userList) {
    String sql = "insert into user values (?,?,?)";
    int[] ints = jdbcTemplate.batchUpdate(sql, userList);
    System.out.println(Arrays.toString(ints));
}
```

```java
public void appFillUser() {
    ApplicationContext context = new ClassPathXmlApplicationContext("spring-config.xml");
    UserService userService = context.getBean("userService", UserService.class);
    
    List<Object[]> userList = new ArrayList<>();
    Object[] user1 = {1009, "test9", "password0"};
    Object[] user2 = {1010, "test10", "password10"};
    Object[] user3 = {1011, "test11", "password11"};
    userList.add(user1);
    userList.add(user2);
    userList.add(user3);
    userService.fillUser(userList);
}
```

# spring事务管理

> 事务管理应位于service层

## 注解方式

- 创建事务管理器,注入属性（数据源）

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
</bean>
```

- 开启服务

```xml
<tx:annotation-driven transaction-manager="transactionManager"/>
```

```java
	@service
    @Transactional
    
    public class UserService {
        public void Money(UserEntity user1, UserEntity user2, int money) {
            userDao.addMoney(user1, money);
            userDao.decreaseMoney(user2, money);
        }
    }

```

**Transactional的参数**

>  传播行为propagation

```java
@Transactional(propagation = Propagation.REQUIRED)
```

- REQUIRED ：如果当前事务内有无事务的方法，调用有事务的方法时，会使用当前有事务方法内部的方法（默认）
- REQUIRED_NEW ：无论是否有有事务方法内的方法是否有事务，在调用时都会创建新的事务

# 事务隔离级别

- 脏毒：未提交事务读取到另一个未提交事务的数据
- 不可重复读：未提交事务读取到另一个已提交事务修改之后的数据（正常情况，两个事务必须都是已经提交的）
- 虚读：一个未提交的事务读取到另一个已提交事务添加的数据

解决办法

```java
isolation = Isolation.REPEATABLE_READ
```

- READ_UNCOMMITTED（读未提交）（默认，会出现三种问题）

- READ_COMMITTED（读已提交）（仅解决脏读）

- REPEATABLE_READ（可重复读）（解决脏读、不可重复读）

- SERIALIZABLE（串行化）（解决脏读、不可重复读、虚读）

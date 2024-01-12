# Springboot集成Mybatis

## mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url"
                          value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&useUnicode=true&characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="Dd112211"/>
            </dataSource>
        </environment>
    </environments>
    <!--Mapper.xml注册-->
    <mappers>
        <mapper resource="com/horizon/dao/UserMapper.xml"/>
        <!--        <mapper class="com.horizon.dao.UserDao"/>-->
    </mappers>
</configuration>
```

## MybatisUtils

```java
public class MybatisUtils {

    private static SqlSessionFactory sqlSessionFactory;

    static {

        try {
            String resource = "org/mybatis/example/mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }

    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }
}
```

## Dao

```java
public interface UserDao {

    /**
     * 获取所有用户
     *
     * @return 返回用户
     */
    List<User> getUserList();
}
```

## DaoImpl改为UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--与Dao对应-->
<mapper namespace="com.horizon.dao.UserDao">
    <!--id方法名-->
    <!--resultType返回类型-->
    <select id="getUserList" resultType="com.horizon.pojo.User">
        select *
        from mybatis.user
    </select>
</mapper>
```

## Pom.xml

资源过滤，防止编译后目录没有resources目录下的文件

```xml
<build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
                <filtering>true</filtering>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/*.xml</include>
                    <include>**/*.properties</include>
                </includes>
                <filtering>true</filtering>
            </resource>
        </resources>
    </build>
```

## Test

```java
class UserDaoTest {

    @Test
    void getUserList() {

        //获取SqlSession对象
        SqlSession sqlSession = MybatisUtils.getSqlSession();

        UserDao userDao = sqlSession.getMapper(UserDao.class);
        List<User> userList = userDao.getUserList();
        if (userList != null) {
            for (User user : userList) {
                System.out.println(user);
            }
        } else {
            System.out.println("null");
        }

        //关闭SqlSession
        sqlSession.close();
    }
}
```

# 基础CRUD

## 条件查询

接口

```java
User getUserById(int id);
```

接口实现xml

```xml
<!--parameterType返回值类型-->
    <select id="getUserById" resultType="com.horizon.pojo.User" parameterType="int">
        select *
        from mybatis.user
        where id = #{id}
    </select>
```

测试

```java
@Test
void getUserById() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
    System.out.println(userMapper.getUserById(1));

    sqlSession.close();
}
```

## 插入

接口

```java
int addUser(User user);
```

接口实现xml

```xml
<insert id="addUser" parameterType="com.horizon.pojo.User">
    insert into mybatis.user(id, name, pwd)
    VALUES (#{id}, #{name}, #{psw})
</insert>
```

测试

```java
@Test
    void addUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        System.out.println(userMapper.addUser(new User("newUser", "psw")));

        //插入需要提交事务
        sqlSession.commit();
        sqlSession.close();
    }
```

`增删改方法需要条事务，否则不成功`

## 改

```xml
<update id="upDataUserPsw" parameterType="com.horizon.pojo.User">
        update mybatis.user
        set pwd = #{psw}
        where id = #{id}
    </update>
```

```java
@Test
    void upDataUserPsw() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        System.out.println(userMapper.upDataUserPsw(new User(1, "newPsw")));

        sqlSession.commit();
        sqlSession.close();
    }
```

## 删

```xml
<delete id="deleteUser" parameterType="int">
        delete
        from mybatis.user
        where id = #{id}
    </delete>
```

```java
 @Test
    void deleteUser() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        System.out.println(userMapper.deleteUser(4));
        sqlSession.commit();
        sqlSession.close();
    }
```

# 参数传入Map

使用Map完成增删改差

Mapper

```java
int addUserWithMap(Map<String, String> map);
```

Xml

```xml
<insert id="addUserWithMap" parameterType="map">
    insert into mybatis.user(name, pwd)
    VALUES (#{username}, #{password})
</insert>
```

test

```java
@Test
    void addUserWithMap() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

        Map<String, String> map = new HashMap<>();
        map.put("username", "mapName");
        map.put("password", "mapPsw");

        System.out.println(userMapper.addUserWithMap(map));

        sqlSession.commit();
        sqlSession.close();
    }
```

# 模糊查询

Mapper

```java
/**
     * 模糊查询
     *
     * @param nameLike 模糊id
     * @return 返回user
     */
    List<User> getUserByNameLike(String nameLike);
```

xml

```xml
<select id="getUserByNameLike" resultType="com.horizon.pojo.User" parameterType="map">
        select *
        from mybatis.user
        where name like #{nameLike}
    </select>
```

test

```java
    @Test
    void getUserByNameLike() {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        System.out.println(userMapper.getUserByNameLike("%name%"));

    }
```

模糊查询，传递字符串时需要在字符左右加通配符%

# 实体类别名

Mybatis-config.xml

```xml
<typeAliases>
        <typeAlias type="com.horizon.pojo.User" alias="User"/>
    </typeAliases>
```

或者

扫描，默认首字母小写

```xml
<typeAliases>
    <!--        <typeAlias type="com.horizon.pojo.User" alias="User"/>-->
    <package name="com.horizon.pojo"/>
</typeAliases>
```

`扫描包时`，可以在实体类类上加注解`@Alias("User")`，用来制定别名

# Mappers

配置方式

```xml
    <mappers>
        <mapper resource="com/horizon/dao/UserMapper.xml"/>
    </mappers>
```

使用class时 类名必须相同，必须在同一包下

```xml
<mapper class="com.horizon.dao.UserMapper"/>
```

## 结果集映射

当实体类字段与数据库不同名时使用

```xml
<!--结果集映射-->
<!--column数据库名，property实体类名-->
<resultMap id="UserMap" type="User">
    <result column="id" property="id"/>
    <result column="name" property="name"/>
    <result column="pwd" property="password"/>
</resultMap>
```

# 日志

mybatis-config.xml

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

log4j.properties

```properties
log4j.rootLogger=DEBUG,console,file
log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.Target=System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=[%c]-%m%n
log4j.appender.file=org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/horizon.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%-5p [%t] %d{yyyy-MM-dd HH:mm:ss,SSS} %37c %3x - %m%n
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

# 注解开发

mybatis-config.xml

```xml
<mappers>
    <!--        <mapper resource="com/horizon/dao/UserMapper.xml"/>-->
    <mapper class="com.horizon.dao.UserMapper"/>
</mappers>
```

Dao接口

```java
/**
 * 获取所有用户
 *
 * @return 返回List<User></>
 */
@Select("select * from mybatis.user")
List<User> getUserList();

/**
 * 根据id查询
 *
 * @param id int id
 * @return 返回User
 */
@Select("select * from mybatis.user where id = #{id}")
User getUserById(@Param("id") int id);
```

# 多表查询

## 多对一

```JAVA
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Country {
    private int id;
    private String country;
    private User user;
}
```

嵌套查询

```XML
<select id="getCountryAndUser" resultMap="CountryAndUser">
        select *
        from mybatis.country
    </select>
    <!--结果集映射-->
    <resultMap id="CountryAndUser" type="Country">
        <result property="id" column="id"/>
        <result property="country" column="country"/>
        <!--通过userid嵌套查询User-->
        <association property="user" column="userid" javaType="User" select="getUser"/>
    </resultMap>
    <!--查询User对象-->
    <select id="getUser" resultType="User">
        select *
        from mybatis.user
        where id = #{id}
    </select>
```

按照结果嵌套查询

```xml
<select id="getCountryAndUser2" resultMap="CountryAndUser2">
    select user.id as uid, user.name as uname, country.country as country
    from mybatis.user,
         mybatis.country
    where country.userid = user.id
</select>
<resultMap id="CountryAndUser2" type="Country">
    <result property="country" column="country"/>
    <association property="user" javaType="User">
        <result property="id" column="uid"/>
        <result property="name" column="uname"/>
    </association>
</resultMap>
```

## 一对多

pojo

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Asset {
    private int id;
    /**
     * 一个home对应两个user
     */
    private List<User> users;
    private String home;
}
```

```xml
<select id="getCountryAndUser2" resultMap="CountryAndUser2">
    select user.id as uid, user.name as uname, country.country as country
    from mybatis.user,
         mybatis.country
    where country.userid = user.id
</select>
<resultMap id="CountryAndUser2" type="Country">
    <result property="country" column="country"/>
    <association property="user" javaType="User">
        <result property="id" column="uid"/>
        <result property="name" column="uname"/>
    </association>
</resultMap>
```

## 多对一

pojo

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Asset {
    private int id;
    /**
     * 一个home对应两个user
     */
    private List<User> users;
    private String home;
}
```

pojo

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
@Alias("User")
public class User {
    private int id;
    private String name;
    private String pwd;
}
```

xml

```xml
<!--多对一查询-->
<select id="getUserByAsset" resultMap="UserAndAsset">
    select user.id uid, user.name uname, asset.home a_home
    from mybatis.user,
         mybatis.asset
    where asset.userid = user.id
      and asset.id = #{id}
</select>

<resultMap id="UserAndAsset" type="Asset">
    <!--这里type与接口中返回类型相同-->
    <result property="home" column="a_home"/>
    <!--写完这个结果中所有的字段-->
    <collection property="users" ofType="User">
        <!--这里的property与Asset中List<User>名相同-->
        <result property="id" column="uid"/>
        <result property="name" column="uname"/>
    </collection>
</resultMap>
```

另一种方式

```xml
<select id="getUserByAsset2" resultMap="UserAndAsset2">
    select *
    from mybatis.asset
    where id = #{id}
</select>

<resultMap id="UserAndAsset2" type="Asset">
    <collection property="users" javaType="ArrayList" ofType="User" select="getUserById" column="id"/>
</resultMap>

<select id="getUserById" resultType="User">
    select *
    from mybatis.user
    where id = #{id}
</select>
```

> 返回集合使用collection，返回对象使用association

# 动态sql

## 查询

> 注意使用where标签
> 
> 使用where标签，而不是使用where 1 = 1来拼接

xml

```xml
<select id="queryBlogListIf" resultMap="BlogMapper">
    select *
    from mybatis.blog
    <where>
        <if test="title != null">
            and title = #{title}
        </if>
        <if test="views != null">
            and views > #{views}
        </if>
    </where>
</select>
```

```xml
<select id="queryBlogWhen" resultMap="BlogMapper">
    select *
    from mybatis.blog
    <where>
        <choose>
            <when test="title != null">
                and title = #{title}
            </when>
            <when test="author != null">
                and author = #{author}
            </when>
            <when test="views != null">
                and views > #{views}
            </when>
            //当前三个条件都未执行时，自动执行第四条
            <otherwise>
                and id = 1
            </otherwise>
        </choose>
    </where>
</select>
```

## updata

```xml
<update id="changeInfo">
        update mybatis.blog
        <set>
            <if test="title != null">
                title = #{title},
            </if>
            <if test="author != null">
                author = #{author},
            </if>
            <if test="views != null">
                views = #{views}
            </if>
        </set>
        where id = #{id}
    </update>
```

>  需要传入title author views中至少一个，必须传入id
> 
> if标签里字段后面的逗号不能省

### foreach

```xml
<select id="queryBlogListByForeach" resultMap="BlogMapper">
        select * from mybatis.blog
        <where>
            /*collection map中的key
            item对应id
            open是sql的开始
            close是sql的结束
            separator是拼接的分隔符
            */
            <foreach collection="idList" item="id" open="and (" close=")" separator="or">
                id = #{id}
            </foreach>
        </where>
    </select>
```

数据库

![image-20220425151652231](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220425151652231.png)

# Mybatis缓存

默认情况只启用一级本地缓存

> **Fifo**先进先出原则：按对象进入缓存的顺序移除缓存
> 
> **lru**最近最少使用原则：移除最长时间不被使用的对象

什么情况下缓存失效

1. 查询不同内容
2. 增删改刷新缓存
3. 查询不同的Mapper.xml
4. 手动清理

**二级缓存**

一级缓存随之会话的关闭回丢失

开启之后会将一级缓存中的内容保存包二级缓存

不同mapper缓存分离

# 其他

## #{}和${}区别

- #{}是预编译处理，${}是字符串替换
- #{}在编译时会被替换为 ? 占位符，在使用PreparedStatement的set()方法来赋值
- ${}会被直接替换成字符串的值
- 使用#{}可以防止SQL注入

## Mybatis中的标签

- `<select> <insert> <update> <delete>`
- `<resultMap> <parameterMap>`结果集映射，参数类型
- `<sql> <include>`sql片段，引入固定sql片段
- `<selectKey>`不支持自增的主键生成策略
- `<trim> <where> <set> <foreach> <if> <choose> <when> <otherwise> <bind>` 动态SQL

## Dao接口工作原理

xml中通过namespace与dao接口对应，通过dao接口的方法名字符串与MappedStatement中id值对应

## Dao中方法是否可以重载

Dao中方法可以重载，但id不可重复，可以使用动态SQL实现。

## Mybatis如何分页

- Mybatis使用RowRounds对象进行分页，是针对ResultSet结果集执行的内存分页，而不是物理分页。
- 可以在SQL中使用动态SQL进行物理分页
- 使用分页插件分页

## Mybatis插件运行原理

可以针对`ParameterHandler`、`ResultSetHandler`、`StatementHandler`、`Executor`四种接口使用插件。

使用AOP的方式拦截`InvocationHandler`的`invoke()`方法。

具体：实现Mybatis `Interceptor` 接口，重写 `intercept()` 方法，之后确定要拦截哪个几口，并在配置文件中配置插件。

## 动态SQL

> [动态SQL](https://segmentfault.com/a/1190000039335704)

9中动态SQL标签：

- `<if>`
- `<whrer> <trim> <set>`
- `<choose> <when> <otherwise>`
- `<foreach>`
- `<bind>`

## Executor 执行器

- `SimplerExecutor`每执行一次update或select就开启一个statement对象，用完立即关闭statement对象。
- `ReuseExecuor`statement对象可复用，执行update或select，以sql作为key查找statement对象，存在就使用，不存在则创建，使用完之后放入Map中，供下一次使用。
- `BatchExecutor`批处理，执行update时，将sql添加到批处理中，等待统一执行。

可以在配置文件中指定默认的`ExecutorType`执行器类型，也可以手动给`DefaultSqlSessionFactory`创建SqlSession的方法传递`ExecutorType`类型参数。

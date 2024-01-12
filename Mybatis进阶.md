## MyBatis

MyBatis是一个持久层框架，内部封装了JDBC，使用MyBatis省去了使用JDBC是的加载驱动，创建链接，创建statement等步骤。

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

- <if>
- <whrer> <trim> <set>
- <choose> <when> <otherwise>
- <foreach>
- <bind>

## JDBC步骤

- 加载驱动程序
- 获取数据库连接
- 创建Statement对象
- 执行SQL
- 获取结果
- 关闭数据库连接

## Executor 执行器

- `SimplerExecutor`每执行一次update或select就开启一个statement对象，用完立即关闭statement对象。
- `ReuseExecuor`statement对象可复用，执行update或select，以sql作为key查找statement对象，存在就使用，不存在则创建，使用完之后放入Map中，供下一次使用。
- `BatchExecutor`批处理，执行update时，将sql添加到批处理中，等待统一执行。

可以在配置文件中指定默认的`ExecutorType`执行器类型，也可以手动给`DefaultSqlSessionFactory`创建SqlSession的方法传递`ExecutorType`类型参数。

## MyBatis分页

MyBatis可以对4个对象进行拦截

- Executer：执行器
- StatementHandler：执行SQL的过程
- ParameterHandler：参数组装
- ResultSetHandler：结果集封装

通过对StatementHandler拦截，拼接上limit字段，从而实现分页




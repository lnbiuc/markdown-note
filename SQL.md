## 建表

```sql
create table <表名>
(
	<列名> <数据类型>(长度) PRIMARY KET,#主键
    <列名> <数据类型>(长度) UNIQUE,#唯一
    <列名> <数据类型>(长度) DEFAULT ' '#默认值
    FOREIGN KEY (<列名>) REFERENCES <表名>(列名),#外键
)
```

## 数据插入

```sql
INSERT INTO classsql.students (学号, 姓名, 性别, 出生年份, 籍贯, 学院) 
VALUES ('091507', '王某某', '男', 2001, '北京', '电信')
```

## 修改

```sql
UPDATE classsql.students t 
SET t.姓名 = '李某某' 
WHERE t.学号 
LIKE '091507' 
ESCAPE '#'
```

```sql
UPDATE scores
set scores.`成绩` = 33
WHERE scores.`学号` = 
(SELECT students.`学号`
FROM students
WHERE students.`学号` = scores.`学号`
AND students.`学院` = "电信")
```





## 删除行

```sql
DELETE FROM classsql.students 
WHERE 学号 
LIKE '091507' 
ESCAPE '#'
```

```sql
DELETE FROM table_name
```

```sql
DELETE * FROM table_name
```

## 删除表

```sql
DROP TABLE table_name
```

## 查看接收到数据数目

```sql
int count = 0;
            while (rs.next()) {
                count = rs.getInt(1);
            }
            System.out.println("接收到 " + count + " 条数据");
            
```

## 使用PreparedStatement防止SQL注入

```java
String sql = "insert into hero values(null,?,?,?)";
PreparedStatement ps = c.prepareStatement(sql);
ps.setString(1,value);
ps.setString(2,value);
ps.setString(3,value);
ps.exectue();
```

## execute

- `execute**()`与`executeUpdate()`

    - 相同点：
        - 都可以执行增加，删除，修改

    - 不同点
        - execute**可以执行查询语句**
            然后通过getResultSet，把结果集取出来
            executeUpdate**不能执行查询语句**
        - execute**返回boolean类型**，true表示执行的是查询语句，false表示执行的是insert,delete,update等等
            executeUpdate**返回的是int**，表示有多少条数据受到了影响

​		

## 获取数据库属性

```java
public class TestJDBC {
  
    public static void main(String[] args) throws Exception {
        try {
            Class.forName("com.mysql.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
 
        try (Connection c = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/how2java?characterEncoding=UTF-8","root", "admin");) {
  
            // 查看数据库层面的元数据
            // 即数据库服务器版本，驱动版本，都有哪些数据库等等
  
            DatabaseMetaData dbmd = c.getMetaData();
  
            // 获取数据库服务器产品名称
            System.out.println("数据库产品名称:\t"+dbmd.getDatabaseProductName());
            // 获取数据库服务器产品版本号
            System.out.println("数据库产品版本:\t"+dbmd.getDatabaseProductVersion());
            // 获取数据库服务器用作类别和表名之间的分隔符 如test.user
            System.out.println("数据库和表分隔符:\t"+dbmd.getCatalogSeparator());
            // 获取驱动版本
            System.out.println("驱动版本:\t"+dbmd.getDriverVersion());
  
            System.out.println("可用的数据库列表：");
            // 获取数据库名称
            ResultSet rs = dbmd.getCatalogs();
  
            while (rs.next()) {
                System.out.println("数据库名称:\t"+rs.getString(1));
            }
  
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
  
    }
}
```


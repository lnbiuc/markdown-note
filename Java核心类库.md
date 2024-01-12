# 字符串和编码

## String

- Stirng是一个引用类型。

- Stirng的本质是通过一个`char[]`数组表示

`String stirng = new String(new char [] {'h','e','l','l','o'})`

- 字符串不可变，字符串内部通过`private final char[]`修饰

### 字符串比较必须使用`equals()`

表面上看使用`==`和`equals()`结果相同，是因为在编译时，jvm后自动把所有相同的字符串当作一个对象，相同的字符串引用相同

### String常用方法

- 去除首位空白字符`"string".trin()`
- 替换`string.replace("2","3")`
- 分割字符串`string.split("正则表达式")`
- 拼接字符串`string.join(" ",string2)`
- 类型转换`valueOf()`

### 总结

- Java字符串`String`是不可变对象；
- 字符串操作不改变原字符串内容，而是返回新字符串；
- 常用的字符串操作：提取子串、查找、替换、大小写转换等；
- Java使用Unicode编码表示`String`和`char`；
- 转换编码就是将`String`和`byte[]`转换，需要指定编码；
- 转换为`byte[]`时，始终优先考虑`UTF-8`编码。

## StringBuilder

通常情况下，每次修改字符串实际上都创建了新的字符串。

使用StringBuilder不会创建新的字符串

#### 使用StringBuilder修改字符串

```java
StringBuilder sb = new StringBuilder(1024);
for (int i = 0; i < 1000; i++) {
    sb.append(',');
    sb.append(i);
}
String s = sb.toString();
```

### 常用方法

- `.insert(3," ")`在指定位置插入指定内容

### 总结

- `StringBuilder`是可变对象，用来高效拼接字符串；

- `StringBuilder`可以支持链式操作，实现链式操作的关键是返回实例本身；

- `StringBuffer`是`StringBuilder`的线程安全版本，现在很少使用 

## StringBuffer

默认空str缓冲区容量16

添加内容之后自动添加`(.length+1)*2`个容量

## StringJoiner

### 总结

- 用指定分隔符拼接字符串数组时，使用`StringJoiner`或者`String.join()`更方便；

- 用`StringJoiner`拼接字符串时，还可以额外附加一个“开头”和“结尾”。

## 包装类型

### 常用方法

静态方法`parseInt()`可以把字符串解析成一个整数：

```java
public class Main {
    public static void main(String[] args) {
        int x1 = Integer.parseInt("100"); // 100
int x2 = Integer.parseInt("100", 16); // 256,因为按16进制解析
        System.out.println(Integer.toString(100)); // "100",表示为10进制
        System.out.println(Integer.toString(100, 36)); // "2s",表示为36进制
        System.out.println(Integer.toHexString(100)); // "64",表示为16进制
        System.out.println(Integer.toOctalString(100)); // "144",表示为8进制
        System.out.println(Integer.toBinaryString(100)); // "1100100",表示为2进制
    }
}

```



### 总结

- Java核心库提供的包装类型可以把基本类型包装为`class`；

- 自动装箱和自动拆箱都是在编译期完成的（JDK>=1.5）；

- 装箱和拆箱会影响执行效率，且拆箱时可能发生`NullPointerException`；

- 包装类型的比较必须使用`equals()`；

- 整数和浮点数的包装类型都继承自`Number`；

- 包装类型提供了大量实用方法。

## JavaBean

### 总结

- JavaBean是一种符合命名规范的`class`，它通过`getter`和`setter`来定义属性；

- 属性是一种通用的叫法，并非Java语法规定；

- 可以利用IDE快速生成`getter`和`setter`；

- 使用`Introspector.getBeanInfo()`可以获取属性列表。

## 枚举类

### 如何理解枚举类

```java
public enum Color {
    RED, GREEN, BLUE;
}
```

编译之后文件

```java
public final class Color extends Enum { // 继承自Enum，标记为final class
    // 每个实例均为全局唯一:
    public static final Color RED = new Color();
    public static final Color GREEN = new Color();
    public static final Color BLUE = new Color();
    // private构造方法，确保外部无法调用new操作符:
    private Color() {}
}
```



```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        if (day == Weekday.SAT || day == Weekday.SUN) {
            System.out.println("Work at home!");
        } else {
            System.out.println("Work at office!");
        }
    }
}

enum Weekday {
    SUN, MON, TUE, WED, THU, FRI, SAT;
}

```

### 枚举类和普通类没有区别

- 定义的`enum`类型总是继承自`java.lang.Enum`，且无法被继承；
- 只能定义出`enum`的实例，而无法通过`new`操作符创建`enum`的实例；
- 定义的每个实例都是引用类型的唯一实例；
- 可以将`enum`类型用于`switch`语句。

### 枚举类适合搭配switch使用

```java
public class Main {
    public static void main(String[] args) {
        Weekday day = Weekday.SUN;
        switch(day) {
        case MON:
        case TUE:
        case WED:
        case THU:
        case FRI:
            System.out.println("Today is " + day + ". Work at office!");
            break;
        case SAT:
        case SUN:
            System.out.println("Today is " + day + ". Work at home!");
            break;
        default:
            throw new RuntimeException("cannot process " + day);
        }
    }
}

enum Weekday {
    MON, TUE, WED, THU, FRI, SAT, SUN;
}
```

## 记录类 Java14特性

### 总结

从Java 14开始，提供新的`record`关键字，可以非常方便地定义Data Class：

- 使用`record`定义的是不变类；
- 可以编写Compact Constructor对参数进行验证；
- 可以定义静态方法。

## BigInteger

模拟一个范围超过long的整数

对`BinInteger`运算时，只能使用实例方法

```java
BigInteger i1 = new BigInteger("1234567890");
BigInteger i2 = new BigInteger("12345678901234567890");
BigInteger sum = i1.add(i2); // 12345678902469135780
```

### 总结

- `BigInteger`用于表示任意大小的整数；

- `BigInteger`是不变类，并且继承自`Number`；

- 将`BigInteger`转换成基本类型时可使用`longValueExact()`等方法保证结果准确。

## BigDecimal（十进制）

`BigDecimal`可以表示一个任意大小且精度完全准确的浮点数。

`BigDecimal`用`scale()`表示小数位数

```java
BigDecimal d1 = new BigDecimal("123.45");
BigDecimal d2 = new BigDecimal("123.4500");
BigDecimal d3 = new BigDecimal("1234500");
System.out.println(d1.scale()); // 2,两位小数
System.out.println(d2.scale()); // 4
System.out.println(d3.scale()); // 0
```

### 比较两个`BigDecimal`

> 总是使用compareTo()比较两个BigDecimal的值，不要使用equals()！

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("123.45600");
System.out.println(d1.equals(d2)); // false,因为scale不同
System.out.println(d1.equals(d2.stripTrailingZeros())); // true,因为d2去除尾部0后scale变为2
System.out.println(d1.compareTo(d2)); // 0
```

### 总结

- `BigDecimal`用于表示精确的小数，常用于财务计算；

- 比较`BigDecimal`的值是否相等，必须使用`compareTo()`而不能使用`equals()`。

## 常用工具类

### `Math`

求绝对值：

```java
Math.abs(-100); // 100
Math.abs(-7.8); // 7.8
```

取最大或最小值：

```java
Math.max(100, 99); // 100
Math.min(1.2, 2.3); // 1.2
```

计算xy次方：

```java
Math.pow(2, 10); // 2的10次方=1024
```

计算√x：

```java
Math.sqrt(2); // 1.414...
```

计算ex次方：

```java
Math.exp(2); // 7.389...
```

计算以e为底的对数：

```java
Math.log(4); // 1.386...
```

计算以10为底的对数：

```java
Math.log10(100); // 2
```

三角函数：

```java
Math.sin(3.14); // 0.00159...
Math.cos(3.14); // -0.9999...
Math.tan(3.14); // -0.0015...
Math.asin(1.0); // 1.57079...
Math.acos(1.0); // 0.0
```

Math还提供了几个数学常量：

```java
double pi = Math.PI; // 3.14159...
double e = Math.E; // 2.7182818...
Math.sin(Math.PI / 6); // sin(π/6) = 0.5
```

生成一个随机数x，x的范围是`0 <= x < 1`：

```java
Math.random(); // 0.53907... 每次都不一样
```

### 生成特定范围随机数

```java
// 区间在[MIN, MAX)的随机数
public class Main {
    public static void main(String[] args) {
        double x = Math.random(); // x的范围是[0,1)
        double min = 10;
        double max = 50;
        double y = x * (max - min) + min; // y的范围是[10,50)
        long n = (long) y; // n的范围是[10,50)的整数
        System.out.println(y);
        System.out.println(n);
    }
}
```



### 上面的是假随机数，这个才是真的SecureRandom

`SecureRandom`就是用来创建安全的随机数的：

```java
SecureRandom sr = new SecureRandom();
System.out.println(sr.nextInt(100));
```

## HashSet

> 实现set接口，元素不可重复，无序存储

## TreeSet

> 实现set接口，有序不可重复存储

## 集合框架

![image-20220420163957340](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220420163957340.png)

![image-20220420173818406](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220420173818406.png)

## Servlet生命周期

![image-20220422152939671](https://typora-1308549476.cos.ap-nanjing.myqcloud.com/img/image-20220422152939671.png)
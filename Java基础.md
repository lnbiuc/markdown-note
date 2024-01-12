数组
==

定义数组
----

`int[] array = new array[n];`

### 数组操作

#### 写入元素

1. `array[0] = 10`
 `array[1] = 20`
    `array[2] = 30`
2. 使用`Arrays.fill()`直接写入相同元素
 给整个数组填充
    `Array.fill(array,100)`
    覆盖数组中i-j位置第内容
    `Array.fill(array.i.j.100)`
3. 静态初始化数组
 `int[] array = {1,2,3,4}`

#### 排序

`sort()`

#### 查找特定元素

`binarySearch()`返回给元素在数组中的位置

#### 获取数组长度

`array.length()`

#### 获取最小值和最大值

`Collection.max()`
 `Collection.min()`

#### 数组合并

`list.Addall(array1.asList(array2)`

动态数组
====

定义动态数组
------

`ArrayList array = new ArrayList`

### 动态数组操作

#### 添加元素

`array.add()`

#### 删除元素

`array.clear()`

#### 计算两数组的差集

`Array1.removeAll(Array2)`

#### 计算两数组的补集

`Array1.retainAll(Array2)`

#### 查找特定元素，返回true或false

`array.contains ()`

#### 判断两数组是否相等，返回true或false

`Array1.equals (Array2)`

二维数组
====

定义二维数组
------

`int array[] [] = new int [] [] { {1,2,3},{4,5,6},{7,8,9} }`

二维数组操作示例
--------

```java
public class DoubleArray {
public static void main(String[] args) {
int[][] array = new int[][]{{1,24},{2,35,4},{4,57,60,7},{7,8,9,}};
System.out.print("遍历：");
ergodicAll(array);
System.out.println(" ");
System.out.println("max: "+getMax(array));
System.out.println("min: "+getMin(array));
System.out.println("sum: "+getSum(array));
System.out.println("average: "+getAverage(array));
}
    //遍历
    public static void ergodicAll(int[][] array0) {
        for (int i=0;i<array0.length;i++){
            for (int j=0;j<array0[i].length;j++){
                System.out.print(array0[i][j]+" ");
            }
        }
    }
    //获取最大值
    public static int getMax(int[][] array1) {
        int max = array1[0][0];
        for (int i=0;i<array1.length;i++){
            for (int j=0;j<array1[i].length;j++){
                if (array1[i][j]>max){
                    max = array1[i][j];
                }
            }
        }
        return max;
    }
    //获取最小值
    public static int getMin(int[][] array2){
        int min = array2[0][0];
        for (int i=0;i<array2.length;i++){
            for (int j=0;j<array2[i].length;j++){
                if (array2[i][j]<min){
                    min = array2[i][j];
                }
            }
        }
        return min;
    }
    //求所有元素之和
    public static int getSum(int[][] array3){
        int sum = 0;
        int count = 0;
        for (int i=0;i<array3.length;i++){
            for (int j=0;j<array3[i].length;j++){
                sum = sum+array3[i][j];
            }
        }
        return sum;
    }
    //求所有元素平均值
    public static float getAverage(int[][] array4){
        float average = 0;
        float count = 0;
        for (int i=0;i<array4.length;i++){
            for(int j=0;j<array4[i].length;j++){
                count = count + 1;
            }
        }
        average = getSum(array4)/count;
        return average;
    }
}

```

Java修饰符
=======

访问控制修饰符
-------

#### public 对所有类可见

#### private （私有的）同一类中可见

#### default 同一package中可见

#### protected 同一包内的类和所有子类可见

非访问修饰符
------

### static

关键字用来声明独立于对象的静态变量，无论一个类实例化多少对象，它的静态变量只有一份拷贝。 静态变量也被称为类变量。局部变量不能被声明为 static 变量。

### final 修饰符

变量一旦赋值后，不能被重新赋值。被 final 修饰的实例变量必须显式指定初始值
 final类不能被继承

### abstract修饰符

抽象类不能用来实例化对象，声明抽象类的唯一目的是为了将来对该类进行扩充

赋值运算符
=====

1. += `C += A 等价于 C = C + A`
2. -+ `C -= A 等价于 C = C - A`
3. \*= `C *= A 等价于 C = C * A`
4. /= `C /= A 等价于 C = C / A`

switch case 语句
==============

switch case 语句判断一个变量与一系列值中某个值是否相等，每个值称为一个分支。

```java
public class Test {
   public static void main(String args[]){
      //char grade = args[0].charAt(0);
      char grade = 'C';

      switch(grade)
      {
         case 'A' :
            System.out.println("优秀"); 
            break;
         case 'B' :
         case 'C' :
            System.out.println("良好");
            break;
         case 'D' :
            System.out.println("及格");
            break;
         case 'F' :
            System.out.println("你需要再努力努力");
            break;
         default :
            System.out.println("未知等级");
      }
      System.out.println("你的等级是 " + grade);
   }
}

```

StringBuilder
=============

```java
package StringBuffer;

/**
 * @author ayanamirei
 * StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder
 * 的方法不是线程安全的（不能同步访问）。
 * 由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。
 */
public class Test {
    public static void main(String[] args){
        //创建容量为32
        StringBuilder string = new StringBuilder(64);
        string.append("This is a string!");
        string.append(123);
        string.append(1.3);
        //在固定位置添加字段
        string.insert(12,"newString");
        //删除固定位置当字段
        string.delete(10,20);
        //反转
        string.reverse();
        //替换
        string.replace(1,2,"new");
        //返回容量
        System.out.println(string.capacity());
        System.out.println(string);
    }
}

```

方法重载
====

作用
--

如果一个类中具有多个相同名称但参数不同的方法，称为方法重载

### 例如

假设必须执行给定数值的添加操作(求和)，但是参数的数量
 不固定，如果为两个参数编写add1(int，int)方法，
 为三个参数编写add2(int，int，int)方法.可以用重载。

```java
public class Employee {
    String name;
    int age;
    String designation;
    double salary;
    //Employee 类的构造器
    public Employee(String name){
        this.name = name;
    }
    //设置age的值
    public void empAge(int empAge){
        age =  empAge;
    }
    //设置designation的值
    public void empDesignation(String empDesig){
        designation = empDesig;
    }
    //设置salary的值
    public void empSalary(double empSalary){
        salary = empSalary;
    }
    //打印信息
    public void printEmployee(){
        System.out.println("名字:"+ name );
        System.out.println("年龄:" + age );
        System.out.println("职位:" + designation );
        System.out.println("薪水:" + salary);
    }
}

```

从键盘获取内容
=======

Scanner类
--------

### next()不计算空格

```java
public class inPutTest0 {
    public static void main(String[] args){
        Scanner data = new Scanner(System.in);
        //通过next方法接收
        if (data.hasNext()){
            String s1 = data.next();
            System.out.println(s1);
        }
        data.close();
    }
}

```

### nextLine()可以包括空格

```java
public class inPutTest1 {
    public static void main(String[] args){
        Scanner data = new Scanner(System.in);
        //通过nextLine接收
        if (data.hasNext()){
            String s1 = data.nextLine();
            System.out.println(s1);
        }
        data.close();
    }
}

```

多态（Polymorpic）
==============

什么是多态
-----

### 同时存在以下三个条件

1.有继承关系；2.有方法重写；3.父类应用指向子类对象

### 多态中成员访问

#### 访问成员变量

```java
class Father{
		int num = 10;
}
class Son extends Father{
		int num = 20;
}
public static void main(String[] args){
		Father f = new Son();
		System.out.println("f.num")
}
=======
out:10

```

编译时看`Father f = new Son();`左边，`Father`中无错误即编译通过

运行时看左边，只访问`Father`中内容

#### 访问成员方法

```java
public class Duotai {
    public static void main(String[] args){
        Father f = new Son();
        System.out.println(f.num);
        f.print();
        Father.method();
    }
}
class Father{
    int num = 10;
    public void print(){
        System.out.println("Father");
    }
    public static void method(){
        System.out.println("father static method");
    }
}
class Son extends Father{
    int num = 20;
    @Override
    public void print(){
        System.out.println("Son");
    }
    public static void method(){
        System.out.println("son static method");
    }
}
========
out:
10
Son
father static method

```

编译看左边，运行看右边

#### 静态方法

静态方法编译看左边，运行看左边

抽象类(Abstract)
=============

1. 用于定义变量
2. 抽象类不能被实例化，但可继承

### 定义抽象类示例

```java
public class Abstract {
    public static void main(String[] args){
        Cat c = new Cat();
        c.eat();
        Dog d = new Dog();
        d.eat();
    }
}
abstract class Animal{
    public abstract void eat();
}
class Cat extends Animal{
    @Override
    public void eat(){
        System.out.println("cat eat");
    }
}
class Dog extends Animal{
    @Override
    public void eat(){
        System.out.println("dog eat");
    }
}

```

### 使用继承的方法访问抽象类中的方法

```java
public class Abstract {
    public static void main(String[] args){
        Manager m = new Manager();
        m.name();
        m.number();
        m.wages();
        m.rewardWages();
    }
}
abstract class Staff{
    public abstract void inf();
    public void name(){
        System.out.println("name");
    }
    public void number(){
        System.out.println("number");
    }
    public void wages(){
        System.out.println("wages");
    }
}
class Manager extends Staff{
    public void rewardWages(){
        System.out.println("rewardWages");
    }
    @Override
    public void inf() {
    }
}

```

### 使用继承的方法访问抽象类中的变量

```java
abstract class Abstract {
    int i;
    int a;
    int b;
    boolean j;
}
class Calculation extends Abstract{
    public void inPut(int i,int a,int b){
        this.i = i;
        this.a = a;
        this.b = b;
    }
    public void inPut(int i,int a){
        this.i = i;
        this.a = a;
    }
    public int getSum(){
        return (i + a + b);
    }
    public int getRide(){
        return (i * a * b);
    }
}
class Use extends Abstract{
    public void inPut(int i,int a,int b,boolean j){
        this.i = i;
        this.a = a;
        this.b = b;
        this.j = j;
    }
    public boolean tOrf(){
        j = a > b;
        return j;
    }
}
class App extends Calculation{
    public static void main(String[] args){
        Calculation c1 = new Calculation();
        c1.inPut(2,5);
        System.out.println("Sum: " + c1.getSum());
        System.out.println("Ride: " + c1.getRide());
        System.out.println("=============");
        Calculation c2 = new Calculation();
        c2.inPut(2,3,4);
        System.out.println("Sum: " + c2.getSum());
        System.out.println("Ride: " + c2.getRide());
        System.out.println("------------");
        Use u1 = new Use();
        u1.inPut(1,2,3,false);
        System.out.println("a > b ? " + u1.tOrf());
    }
}

```

### 使用内部类的方法对抽象类中的方法进行重写

```java
public class Demo2 {
    public static void main(String[] args) {
        //3实例化
        Abstract1Use abstract1Use = new Abstract1Use();
        //4调用方法，传递的参数为（抽象类的实例化对象）
        abstract1Use.method(new Abstract1() {
            //5重写抽象类中的方法
            @Override
            void show() {
                System.out.println("RebuildAbstractUse");
            }
        });
    }
}
//1
abstract class Abstract1 {
    abstract void show();
}
//2
class Abstract1Use {
    //2将抽象类的方法的对象以参数的形式传递，从而实现对抽象方法对调用
    public void method(Abstract1 abstract1) {
        abstract1.show();
    }
}

```

说明：

1. 写一个抽象类
2. 写一个App类，将抽象类的示例化对象以参数的形式传入，调用抽象类中的方法
3. 实例化App类
4. 调用App类中的方法，传入的参数为，对抽象类的实例化创建`new abstract`
5. 同时包含对于抽象类中方法的重写

### 抽象类的用法

1. 创建抽象类—正常访问抽象类中的成员变量及成员方法
2. 创建抽象类—创建另一个类继承抽象类—使用`super()`获取抽象类中的成员变量—正常使用抽象类中的成员变量—创建类的实例

### 抽象方法

写在抽象类中，例`public abstract void 方法名();`

总结
--

* 抽象方法不能被示例化
* 抽象类中不一定包含抽象方法，但有抽象方法的类必须被定义为抽象类
* 构造方法，类方法(由`static`修饰的方法)不能声明为抽象方法
* 抽象类的子类必须给出抽象类中的抽象方法的具体实现，除非该子类也是抽象类

接口（Interface）
=============

* 接口抽象方法的集合，接口中所有的方法必须为抽象方法
* 接口中的所有方法必须为`public abstract`
* 接口中的变量一定是`public static final`
* 接口没有构造方法
* 接口不是类，类描述对象的属性和方法，而接口包含要实现的方法
* 实现接口的类中，实现接口的方法，必须实现接口中的所有方法。否则该类必须为抽象类

### 声明一个接口

```java
[可见度] interface 接口名称 [extends 其他的接口名] {
        // 声明变量
        // 抽象方法
}

```

### 接口的实现

```
class [类名] implements [接口名]

```

### 示例

```java
public interface Shape {
    static void main(String[] args){
        UseInter u1 = new UseInter();
        System.out.println("Add: " + u1.getAdd(2,4));
        System.out.println("Ride: " + u1.getRide(2,4));
        System.out.println("max: " + u1.getMax(2,4));
    }
}
interface Inter{
    int getAdd(int a,int b);
    int getRide(int a,int b);
    int getMax(int a,int b);
}
class UseInter implements Inter{
    @Override
    public int getAdd(int a,int b){
        return (a + b);
    }
    @Override
    public int getRide(int a,int b){
        return (a * b);
    }
    @Override
    public int getMax(int a,int b){
        if (a > b){
            return a;
        } else {
            return b;
        }
    }
}

```

### 接口继承

```java
public interface Sports
{
   public void setHomeTeam(String name);
   public void setVisitingTeam(String name);
}

// 文件名: Football.java
public interface Football extends Sports
{
   public void homeTeamScored(int points);
   public void visitingTeamScored(int points);
   public void endOfQuarter(int quarter);
}

// 文件名: Hockey.java
public interface Hockey extends Sports
{
   public void homeGoalScored();
   public void visitingGoalScored();
   public void endOfPeriod(int period);
   public void overtimePeriod(int ot);
}

```

上述示例中，Sports接口有2个方法，Football继承Sports之后需实现5个方法，Hockey继承之后需要实现6个方法

### 接口多继承

`public interface Hockey extends Sports, Event`

### 标记接口

* 标记接口是没有任何属性的接口
* 没有任何方法的接口称为标记接口

### 在方法中写入接口的两种方式

#### 在方法中创建类，实现类中类

方法中应该包含实现类的示例化和对方法的调用

```java
class Outer {
    public void method() {
        class Inner2 implements Inner {
            @Override
            public void print() {
                System.out.println("Inner2 print");
            }
        }
        Inner2 inner2 = new Inner2();
        inner2.print();

```

#### 使用`new 接口名(){方法}`实现接口

```
class Outer {
    public void method() {
        new Inner2() {
            @Override
            public void print() {
                System.out.println("inner2p");
            }
        }.print();

```

#### 先在方法中创建接口的对象，在对象中写入方法，之后调用这个方法

先示例化接口对象
 `接口 接1 = new 接口(){方法};通过对象调用方法`

```java
class App {
    public void method() {
        Print print = new Print() {
            @Override
            public void print() {
                System.out.println("print");
            }
        };
        print.print();
    }
}

```

枚举（enum）
========

### 一个表示常量的类

#### 定义enum类

```
enum color {
		blue,pink,red
}

```

#### 访问enum类中的内容

```java
class Enum{
    public static void main(String[] args){
        color c1 = color.black;
        System.out.println(c1);
    }
}

```

#### 使用for循环访问enum类中的内容

```java
class Enum{
    public static void main(String[] args){
        for (color i : color.values()){
            System.out.println(i);
        }
    }
}

```

枚举中的函数
------

### values()

返回枚举类中所有的值

### ordinal()

找到每个枚举常量的索引

### valoeOf()

返回指定字符串的枚举常量

类
=

### 全类名

### 导入包

内部类
---

### 访问内部类

#### 创建对象

```java
    `In.On o1 = new In().new On();`
```

```java
public class MainApp {
    public static void main(String[] args) {
        Applaction a1 = new Applaction();
        a1.p();
        In i1 = new In();
        i1.print();
        In.On o1 = new In().new On();
        o1.print();
    }
}

class In {
    public void print() {
        System.out.println("out");
    }

    class On {
        public void print() {
            System.out.println("in");
        }
    }
}

```

### 访问内部静态类

静态成员是类的成员

`In.On in = new In.On();`

```java
public class MainApp {
    public static void main(String[] args) {
        Applaction a1 = new Applaction();
        a1.p();
        In i1 = new In();
        i1.print();
        In.On in = new In.On();
        in.print();
    }
}

class In {
    public void print() {
        System.out.println("out");
    }

    static class On {
        public void print() {
            System.out.println("in");
        }
    }
}

```

#### 跨类访问

```java
class MainApp {
    public int num = 10;

    class inner {
        public int num = 20;

        public void show() {
            int num = 30;
            System.out.println(num);//访问当前类num
            System.out.println(this.num);//访问父类num
            System.out.println(inner.this.num);//访问inner的父类的num
        }
    }
}

class Test {
    public static void main(String[] args) {
        MainApp.inner in = new MainApp().new inner();
        in.show();
    }
}

```

局部内部类
-----

在类中创建类，调用这个类的方法会直接执行类中类中的方法

```java
public class Demo {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.method();
        outer.run();
    }
}

class Outer {
    public void method() {
        int num = 10;
        class Inner {
            public void print() {
                System.out.println("Inner: " + num);
            }
        }
        Inner inner = new Inner();
        inner.print();
    }

    public void run() {
        method();
    }
}

```

匿名内部类
-----

在类中创建实现接口的方法，并初始化这个接口和接口中的方法

```java
interface Inner {
    void print();
}

/**
 * @author ayanamirei
 * 在类中创建实现接口的方法，并初始化这个接口和接口中的方法
 */
public class Demo {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.method();
    }
}

class Outer {
    public void method() {
        //实现Inner2接口
        //内部类调用法
        class Inner2 implements Inner {
            @Override
            public void print() {
                System.out.println("Inner2 print");
            }
        }
        Inner2 inner2 = new Inner2();
        inner2.print();
        
        //实现Inner2接口
        //匿名内部类调用法
        new Inner2() {
            @Override
            public void print() {
                System.out.println("inner2p");
            }
        }.print();
    }
}
```

```java
public class Demo2 {
    public static void main(String[] args) {
        //实例化
        Abstract1Use abstract1Use = new Abstract1Use();
        //调用方法，传递的参数为（抽象类的实例化对象）
        abstract1Use.method(new Abstract1() {
            //重写抽象类中的方法
            @Override
            void show() {
                System.out.println("RebuildAbstractUse");
            }
        });
    }
}

abstract class Abstract1 {
    abstract void show();
}

class Abstract1Use {
    //将抽象类的方法的对象以参数的形式传递，从而实现对抽象方法对调用
    public void method(Abstract1 abstract1) {
        abstract1.show();
    }
}
```



#### 静态方法可使用类名调用方法

`类名.静态方法()`

类的高级特性
======

`static`关键字
-----------

### 静态方法和静态变量可以不创建方法直接调用

直接访问class类中的变量名`class.变量名`
 直接访问class类中的方法`class.方法名()`

```java
public class Demo1 {
    public static void main(String[] args) {
        System.out.println(ClassStatic.i);
        ClassStatic.print();
    }
}

class ClassStatic {
    static int i = 1;

    static void print() {
        System.out.println(i);
    }
}

```

### 静态变量和静态方法不能通过实例化的方式调用

![](resources/B23F252778B4E3A962173B05A1BEECEB)

`this`关键字
---------

### `this`访问当前类的变量

style="line-height: 160%; box-sizing: content-box; font-weight: 700; font-size: 41px; border-bottom: 3px double \#999; color: \#000;"\>静态导入 

#### 用于导入指定文件中静态变量或静态方法

#### 如果类中所有方法都是`static`方法，可以使用`import static`导入

#### 导入之后访问其他包中静态方法时，可以不是用类名，直接使用类中的静态方法

`final`关键字
==========

#### `fianl`修饰变量时，变量不能被再次赋值

#### `fianl`修饰方法时，方法不能被重写

#### `fianl`修饰类时，类不能被集成

集合类
===

`set`接口
-------

### `hashset`类

无序，不可重复的数组

`list`接口
--------

### `ArrayList`类

动态数组
 ArrayList类在输出时需要对数据进行`toString()`强制类型转换。
 需要在创建`ArrayList<String>`,`ArrayList<Integer>`时声明其中的数据类型

`map`接口
-------

### `hashmap`类

字典

# 泛型类

#### 在类名后加上`<T>`表明该类为泛型类

##### 泛型类的基本定义

```java
public class Demo0 {
    public static void main(String[] args) {
        GetAndOut<Integer> integerGetAndOut = new GetAndOut<>();
        GetAndOut<String> stringGetAndOut = new GetAndOut<>();
        integerGetAndOut.setValue(12);
        stringGetAndOut.setValue("string");
        
        System.out.println(integerGetAndOut.getT());
        System.out.println(stringGetAndOut.getT());
    }
}

class GetAndOut<T> {
    //初始参数
    private T t;
    //构造函数
    public GetAndOut() {
        
    }
    //set方法
    void setValue(T t) {
        this.t = t;
    }
    //get方法
    T getT() {
        return t;
    }
}
```



或者在方法前加上`<E>`表明该方法为泛型方法

##### 创建简单泛型方法

```java
public class Demo1 {
    public static void main(String[] args) {
        String string1 = "string1";
        String string2 = "string2";
        Integer integer1 = 1;
        Integer integer2 = 0;
        Mot mot = new Mot();
        System.out.println(mot.mot(string1, string2));
        System.out.println(mot.mot(integer1, integer2));
    }
}

class Mot {
    //构造方法
    public Mot() {
    
    }
    
    <T extends Comparable<T>> boolean mot(T t1, T t2) {
        if (t1.compareTo(t2) > 0) {
            return true;
        } else {
            return false;
        }
    }
}
```

##### 泛型方法遍历数组

```java
public class Demo {
    public static void main(String[] args) {
        Integer[] integers = {1, 2, 3, 4, 5, 6};
        Double[] doubles = {12.3, 45.6, 7.6};
        Character[] characters = {'H', 'j', 'f'};
        Create.printArray(integers);
        Create.printArray(doubles);
        Create.printArray(characters);
    }
}

class Create {
    static <E> void printArray(E[] inputArray) {
        for (E i : inputArray) {
            System.out.print(" " + i);
        }
        System.out.println();
    }
}
```

泛型类比较三数大小

##### 使用`<T extends Comparable<T>>`创建方法

```java
public class Demo2 {
    public static void main(String[] args) {
        System.out.println(MaxMinGetWay.maxMinGetWay(1, 2, 4));
        System.out.println(MaxMinGetWay.maxMinGetWay(2.3, 43.5, 23.6));
        System.out.println(MaxMinGetWay.maxMinGetWay("d", "c", "fd"));
    }
}

class MaxMinGetWay {
    static <T extends Comparable<T>> T maxMinGetWay(T x, T y, T z) {
        T max = x;
        if (y.compareTo(max) > 0) {
            max = y;
        }
        if (z.compareTo(max) > 0) {
            max = z;
        }
        return max;
    }
}
```

# `List<E>`接口和`ArratList<E>`方法

##### 将创建的对象以数组的形式保存

使用方法：

1. 实例化
2. 传入值
3. 创建`list`, `List<对象名> list名 = new ArratList<>();`
4. 将实例传入`List`，使用`ListName.add(对象名)`
5. 访问该`Lsit`的方式

```java
for(S i : slist) {
    System.out.println(i)
}
```

# `Map<K,V>`接口和`HashMap<K,V>`类

##### 允许储存两种数据类型的map

`entrySet`返回所有一一键对值

- 创建方法

    ```java
    Map<Integer, String> map = new HashMap<Integer, String>();
    ```

- 使用`put`和`remove`更新

- 遍历方法

    - `for`

        ```java
        for (int i = 1; i < map.size() + 1; i++) {
                    System.out.print(map.get(i) + " ");
                }
        ```

    - `fro`

        ```java
        for (Map.Entry<Integer, String> entry : map.entrySet()) {
                    Integer key = entry.getKey();
                    String value = entry.getValue();
                    System.out.println("key " + key + " value " + value);
                }
        ```

    - 使用迭代器

        ```java
        Collection collection = map.values();
                Iterator iterator = collection.iterator();
                while (iterator.hasNext()) {
                    System.out.println(iterator.next());
                }
        ```

        ​				

        

# Java文件操作

- 获取文件对象

    `File file = new File(pathname)`

    - 其中pathname可取绝对路径或相对路径
    - 相对路径示例`"src/File/Get/file.txt"`	

# Java IO

**如何理解输入输出流**

> 根据流动的方向，分为输入流个输出流

> 根据流动的内容，范围字节流和字符流

> 以程序为基本，流出为输出流；流入为输入流

## InputStream流

通过创建`InputStream`对象来导入一个文件

> `InputStream inputStream = new FileInputStream("src/ReadMe.txt");`

通过`inputstream.read()`读取字节，并返回字节表示的int值（0～225）

读取玩最后一个字节之后返回-1（空格不算）

```java
public class Demo {
    public static void main(String[] args) {
        try {
            FileRead.readFile();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

class FileRead {
    static void readFile() throws IOException {
        InputStream inputStream = new FileInputStream("src/ReadMe.txt");
        for (; ; ) {
            int n = inputStream.read();
            if (n == - 1) {
                break;
            }
            System.out.println(n);
        }
    }
}
```

### 读取会遇到异常

> 使用`throws IOException`抛出异常

常见的异常如：

- 未找到问价
- 没有读取权限

### 读取多个字节到缓冲区，从而提升读取效率

`inputsstream.read(byte[] b)`

每次读取bytes个字符，此处bytes = 10；

```java
class FileRead1 {
    static void readMoreByte() throws IOException {
        InputStream inputStream = new FileInputStream("src/ReadMe.txt");
        byte[] bytes = new byte[10];
        int n;
        while ((n = inputStream.read(bytes)) != - 1) {
            System.out.println("read " + n + " bytes.");
        }
    }
}
```

#### 写一个高端的Hello！

```java
class FileRead3 {
    static void readFile() throws IOException {
        byte[] data = {72, 101, 108, 108, 111, 33};//A 和 空格
        InputStream inputStream = new ByteArrayInputStream(data);
        int n;
        while ((n = inputStream.read()) != - 1) {
            System.out.print((char) n);
        }
    }
}
```

使用`ByteArrayInputStream(data)`读模拟文件

使用`inputStream.read()`读取data数组中的内容

使用`(char) n`将字符转码

## OutputStream流

### 写入文件

```java
class WriteInFile {
    static void writeInFile() throws IOException {
        byte[] bytes = {72, 101, 108, 108, 111, 33};
        OutputStream outputStream = new FileOutputStream("src/WriteMe.txt");
        outputStream.write(bytes);
    }
}
=========
    //hello!
```

将字符串转为字节再写入

使用`"String".getBytes(StandardCharsets.UTF_8)`进行转码

```java
class WriteInFile2 {
    static void writeInfile2() throws IOException {
        OutputStream outputStream = new FileOutputStream("src/WriteMe.txt");
        outputStream.write("what!".getBytes(StandardCharsets.UTF_8));
        outputStream.close();
    }
}
```

### 创建测试

```java
class WriteInFile3 {
    static void writeInFile3() throws IOException {
        byte[] testByte;
        ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
        byteArrayOutputStream.write("hello".getBytes(StandardCharsets.UTF_8));
        byteArrayOutputStream.write("word1".getBytes(StandardCharsets.UTF_8));
        testByte = byteArrayOutputStream.toByteArray();
        System.out.println(new String(testByte, StandardCharsets.UTF_8));
    }
}
```

#### 字节输出流作用是读取文件再输出文件，从而达到重构文件的目的

如何使用字节输出流

1. 使用File类打开一个文件
2. 指定位置
3. 输入输出
4. 关闭

---



# 多线程

## 什么事多线程

> - 多线程，是程序能够同时完成多项任务的拘束
> - 多线程可以是程序同时执行多的执行片段

## 什么是进程

> - 程序一次执行的过程
> - 此过程包括
>     - 进程产生
>     - 发展
>     - 结束

## 什么是线程

>- 线程是比进程更小的执行单元
>- 单个进程执行可以产生多个线程
>- 每个线程有独立的生命周期
>- 同一个进程中，线程共享同样的内存空间
>- 通过共享内存空间完成数据交换，通信，同步
>

## Java线程的机制

> 程序启动时，实际上起启动了一个JVM进程，JVM启动主线程执行`main`方法，在`main`方法中，又可以创建其他线程

## 创建线程的方式

> 创建`Thread`对象来创建一个线程

```java
public class Demo3 {
    public static void main(String[] args) {
        Thread thread = new Thread();
        thread.start();
    }
}
```

![image-20211204232858737](/Users/ayanamirei/Library/Application Support/typora-user-images/image-20211204232858737.png)

> 使用线程池创建一个线程

```java
```



> 通过实现Runnable接口创建线程

```java
public class Demo4 {
    public static void main(String[] args) {
        Thread thread = new Thread(new CreateThread1());
        thread.start();
    }
}

class CreateThread1 implements java.lang.Runnable {
    @Override
    public void run() {
        System.out.println("有线程被创建");
    }
}
```



```java
class Runnable implements java.lang.Runnable {
    private Thread thread;
    private String ThreadName;
    
    Runnable(String name) {
        ThreadName = name;
        System.out.println("创建了线程： " + ThreadName);
    }
```

## 使用`.start()`启动线程

> 线程开始后会马上结束，所以要重写`Thread类`中的`run()`方法

使用**`多态`的方式**，是新创建的线程指向被重写了`run()`方法的`Thread类`

`.start()`会调用 `run()`方法

```java
public class Demo3 {
    public static void main(String[] args) {
        //多态
        Thread thread = new CreateThread();
        thread.start();
    }
}

class CreateThread extends Thread {
    @Override
    public void run() {
        System.out.println("有线程真正运行");
    }
}
```

> 多个线程启动后会并发进行，无法确定先后顺序。可以通过`Thread.sleep(20)`使线程暂时停止运行

## 线程被创建后的状态

> - New：新创建的线程，尚未执行；
> - Runnable：运行中的线程，正在执行`run()`方法的Java代码；
> - Blocked：运行中的线程，因为某些操作被阻塞而挂起；
> - Waiting：运行中的线程，因为某些操作在等待中；
> - Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；
> - Terminated：线程已终止，因为`run()`方法执行完毕。

## 线程终止的原因

> - 线程正常终止：`run()`方法执行到`return`语句返回；
> - 线程意外终止：`run()`方法因为未捕获的异常导致线程终止；
> - 对某个线程的`Thread`实例调用`stop()`方法强制终止（强烈不推荐使用）。

> 一个线程可等待另一个线程，直到其运行结束。

```java
public class Demo6 {
    public static void main(String[] args) {
        Thread thread = new Thread(new CreateThread1());
        System.out.println("start");
        thread.start();
        try {
            thread.join();
            Thread.sleep(10000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("end");
    }
}
```

使用`thread.join()`是线程`main`会等待`thread`运行结束后再结束

使用`Thread.sleep(10000);`可验证这一点

## 中断线程

> 线程在运行时被中断

```java
public class Demo7 {
    public static void main(String[] args) {
        Thread thread = new BuildThread7();
        
        try {
            
            
            thread.start();
            Thread.sleep(50);     //执行 暂定50ms 终止
            thread.interrupt();         //中断线程
            thread.join();              //等待线程结束
            System.out.println("end");
            
            
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class BuildThread7 extends Thread {
    @Override
    public void run() {
        int n = 0;
        while (! isInterrupted()) {
            n++;
            System.out.println(n + "run--");
        }
    }
}
```

> 终止`main`线程，从而终止子线程

如果线程处于等待状态，例如，`t.join()`会让`main`线程进入等待状态，此时，如果对`main`线程调用`interrupt()`，`join()`方法会立刻抛出`InterruptedException`，因此，目标线程只要捕获到`join()`方法抛出的`InterruptedException`，就说明有其他线程对其调用了`interrupt()`方法，通常情况下该线程应该立刻结束运行。

> 通过`volatile变量`设置终止标识位

`boolean running`是一个线程间共享的变量。线程间共享变量需要使用`volatile`关键字标记，确保每个线程都能读取到更新后的变量值。

```java
public class Demo8 {
    public static void main(String[] args) throws InterruptedException {
        BuildThread8 buildThread8 = new BuildThread8();
        buildThread8.start();
        Thread.sleep(10);
        buildThread8.running = false;
    }
}

class BuildThread8 extends Thread {
    public volatile boolean running = true;
    
    @Override
    public void run() {
        int n = 0;
        while (running) {
            n++;
            System.out.println(n + " running");
        }
        System.out.println("end");
    }
}
```

## 守护线程

> 讲一个线程状态设置为守护线程后，jvm会结束进程，不论守护线程是否结束

使用`thread.setDaemon(true)`将一个线程设置为守护线程

## 线程同步

> 线程未同步的例子

```java
public class Demo9 {
    public static void main(String[] args) {
        AddThread addThread = new AddThread();
        DecThread decThread = new DecThread();
        addThread.start();
        decThread.start();
        try {
            addThread.join();
            decThread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Count.count);
    }
}

class Count {
    public static int count = 0;
}

class AddThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100000; i++) {
            Count.count += 1;
        }
    }
}

class DecThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100000; i++) {
            Count.count -= 1;
        }
    }
}
==========
    输出结果不为0
```

> 使用加锁的方式使线程同步执行

`synchronized`保证了代码块在任意时刻最多只有一个线程能执行

```java
public class Demo9 {
    public static void main(String[] args) {
        AddThread addThread = new AddThread();
        DecThread decThread = new DecThread();
        addThread.start();
        decThread.start();
        try {
            addThread.join();
            decThread.join();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Count.count);
    }
}

class Count {
    public static final Object LOCK = new Object();
    public static int count = 0;
}

class AddThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100000; i++) {
            synchronized (Count.LOCK) {
                Count.count += 1;
            }
        }
    }
}

class DecThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100000; i++) {
            synchronized (Count.LOCK) {
                Count.count -= 1;
            }
        }
    }
}
=========
    输出结果一定为0
```

> 使用`synchronized`之后，保证了多线程访问共享变量是的不确定性问题。
>
> 但是使用`synchronized`同样带来性能下降问题，因为`synchronized`代码块不能并发进行，因此会降低程序的执行效率

> 最大化执行效率

1. 只对必须要同步的进程进行同步

- 只对需要的进程上锁。例如对0进行两次+1000和两次-1000。

- 应该将一次+和一次-  分为一组。

- 而不是将 两次+ 和 一次- 分为一组

- 这样便能大大提升效率且不影响运行结果

2. 不需要同步的操作

- 除原子操作外，都不需要同步
- 单行赋值语句

3. 对于`static`方法，是没有`this`实例的，因为`static`方法是针对类而不是实例。但是我们注意到任何一个类都有一个由JVM自动创建的`Class`实例，因此，对`static`方法添加`synchronized`，锁住的是该类的`Class`实例。

## 死锁

> 两个线程各自持有不同的锁，同时试图获取对方的锁，导致死锁

> 死锁发生之后，只能强制结束JVM进程

```java
public class Demo10 {
    public static void main(String[] args) {
        BuildThread10 buildThread10 = new BuildThread10();
        buildThread10.add(10);
        buildThread10.dec(10);
        buildThread10.start();
    }
}

class BuildThread10 extends Thread {
    int value;
    int another;
    
    public void add(int m) {
        synchronized (Lock.LOCKA) {
            this.value += m;
            synchronized (Lock.LOCKB) {
                this.another += m;
            }
        }
    }
    
    public void dec(int m) {
        synchronized (Lock.LOCKB) {
            this.value += m;
            synchronized (Lock.LOCKA) {
                this.another += m;
            }
        }
    }
}

class Lock {
    public static final Object LOCKA = new Object();
    public static final Object LOCKB = new Object();
}
```

线程1和线程2如果分别执行`add()`和`dec()`方法时：

- 线程1：进入`add()`，获得`lockA`；
- 线程2：进入`dec()`，获得`lockB`。

随后：

- 线程1：准备获得`lockB`，失败，等待中；
- 线程2：准备获得`lockA`，失败，等待中。

## 如何避免死锁

> - 使用`wait`来是一个线程进入等待状态
>
> - 使用`notify`唤醒等待状态的线程
>
> - 使用`notifyAll()`将唤醒所有当前正在`this`锁等待的线程，而`notify()`只会唤醒其中一个

>
>
>`wait`和`notify`用于多线程协调运行：
>
>- 在`synchronized`内部可以调用`wait()`使线程进入等待状态；
>- 必须在已获得的锁对象上调用`wait()`方法；
>- 在`synchronized`内部可以调用`notify()`或`notifyAll()`唤醒其他等待线程；
>- 必须在已获得的锁对象上调用`notify()`或`notifyAll()`方法；
>- 已唤醒的线程还需要重新获得锁后才能继续执行。

## 线程池

线程池内部维护了若干个线程，没有任务的时候，这些线程都处于等待状态。如果有新任务，就分配一个空闲线程执行。如果所有线程都处于忙碌状态，新任务要么放入队列等待，要么增加一个新线程进行处理。

##### 创建一个固定大小的线程池

```java
public class Demo12 {
    public static void main(String[] args) {
        ExecutorService executorService = Executors.newFixedThreadPool(3);
        //提交任务
        executorService.submit(Task);
    }
}
```

### 向线程池提交任务

```java
public class Demo12 {
    public static void main(String[] args) {
        ExecutorService es = Executors.newFixedThreadPool(4);
        for (int i = 0; i < 6; i++) {
            es.submit(new Task(" " + i));
        }
        es.shutdown();
    }
}

class Task implements java.lang.Runnable {
    private final String name;
    
    public Task(String name) {
        this.name = name;
    }
    
    @Override
    public void run() {
        System.out.println("start " + name);
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("end " + name);
    }
}
```

创建大小为4的线程池，提交6个任务。

线程池先执行了其中4个，2个处于等待状态，之后再执行2个任务

>- 使用`shutdownNow()`关闭线程池
>- `awaitTermination()`则会等待指定的时间让线程池关闭
>- `CachedThreadPool`线程池可以根据任务数量动态调整大小

### 创建一个可以反复执行的线程池

使用`ScheduledThreadPool`

```java
ScheduledExecutorService ses = Executors.newScheduledThreadPool(4);
```

1s执行1次

```java
ses.schedule(new Task("one-time"), 1, TimeUnit.SECONDS);

```

2秒后开始执行定时任务，每3秒执行

```java
ses.scheduleAtFixedRate(new Task("fixed-rate"), 2, 3, TimeUnit.SECONDS);

```

秒后开始执行定时任务，以3秒为间隔执行

```java
ses.scheduleWithFixedDelay(new Task("fixed-delay"), 2, 3, TimeUnit.SECONDS);
```

# JBDC

## 连接步骤

### 定义常量

```java
public static final String URL = "jdbc:mysql://112.124.44.15:3307/classsql?characterEncoding=UTF-8";
    public static final String USER = "admin";
    public static final String PASSWORD = "zzs12138";
    public static String sql = "select `学号`,`姓名`,`性别` from students";
```

1. 加载驱动器

```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

2. 创建一个连接

```java
Connection connection = DriverManager.getConnection(URL, USER, PASSWORD);
```

3. 创建Statment对象

```java
Statement statement = connection.createStatement();
```

4. 调用statment方法，执行sql语句

```java
statement.execute(sql);
```

5. 创建Resultet对象，用于接收数据

```java
ResultSet rs = statement.executeQuery(sql);
```

6. 处理数据

```sql
while (rs.next()) {
                String num = rs.getString(1);
                String name = rs.getString(2);
                String sex = rs.getString(3);
                System.out.println(num + " " + name + " " + sex);
            }
            System.out.println();
```

7. 关闭连接，释放资源

```java
statement.close();
connection.close();
```


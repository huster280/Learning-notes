

## Java学习路线图

![Java学习路线图](https://pic3.zhimg.com/80/v2-b2794db6eaa03f626485e47f88893f42_720w.jpg)

![](https://img2020.cnblogs.com/blog/1940839/202005/1940839-20200509203642092-919138800.jpg)

- [Java学习路线图](#java学习路线图)
    - [运行Java程序](#运行java程序)
- [Java程序基础](#java程序基础)
    - [类型强制转换](#类型强制转换)
    - [字符串初级操作](#字符串初级操作)
    - [字符串常用函数](#字符串常用函数)
    - [数组](#数组)
    - [格式化输出](#格式化输出)
    - [输入](#输入)
    - [排序](#排序)
- [面向对象基础](#面向对象基础)
    - [引用传递区别](#引用传递区别)
    - [构造方法](#构造方法)
    - [继承](#继承)
    - [多态](#多态)
    - [final关键字](#final关键字)
    - [抽象类](#抽象类)
    - [接口](#接口)
    - [静态字段和静态方法](#静态字段和静态方法)
    - [package和import](#package和import)
    - [classpath和jar](#classpath和jar)
    - [模块](#模块)
- [Java核心类](#java核心类)
    - [StringBuilder](#stringbuilder)
    - [StringJoiner](#stringjoiner)
    - [JavaBean](#javabean)
    - [枚举](#枚举)
    - [常用工具类](#常用工具类)

-   JDK：Java Development Kit（ 开发工具包 ）
-   JRE：Java Runtime Environment（运行字节码的虚拟机）

#### 运行Java程序

 先用`javac`把`Hello.java`编译成字节码文件`Hello.class`，然后，用`java`命令执行这个字节码文件 ；

 可执行文件`javac`是编译器，而可执行文件`java`就是虚拟机 

```ascii
┌──────────────────┐
│    Hello.java    │<─── source code
└──────────────────┘
          │ compile
          ▼
┌──────────────────┐
│   Hello.class    │<─── byte code
└──────────────────┘
          │ execute
          ▼
┌──────────────────┐
│    Run on JVM    │
└──────────────────┘
```

```bash
$ javac Hello.java
$ java Hello
```

## Java程序基础

基本数据类型是CPU可以直接进行运算的类型。Java定义了以下几种基本数据类型：

-   整数类型：byte，short，int，long
-   浮点数类型：float，double（float类型需要加上`f`后缀 ）
-   字符类型：char
-   布尔类型：boolean

引用类型：

-   字符串
-   数组

>   JVM内boolean基本变量4字节（CPU32位寻址，存取操作更方便），boolean数组每个元素1字节。

>    定义变量的时候，如果加上`final`修饰符，这个变量就变成了常量 ：`final double PI = 3.14`

>    **var**关键字` var user = new ArrayList<User>(); `，自动推断变量的类型。



运算操作符与c++基本一样

>   左移`<<`    右移`>>`    与`&`    或`|`    非`~`    异或`^` 
>
>   `>>` 左侧补符号位    `>>>` 左侧补0 
>
>    三元运算符`b ? x : y` 



#### 类型强制转换

>   ```java
>   int n1 = (int)12.3;		//12
>   int n2 = (int) -12.7; 	// -12
>   int n3 = (int) (12.7 + 0.5); 	// 13；加0.5再强制转换相当于四舍五入
>   int n4 = (int) 1.2e20; 	// 2147483647；超过int能表示的最大范围时返回最大值
>   ```



#### 字符串初级操作

**字符串拼接**

-   `concat()`只允许两个字符串之间进行拼接：`s1.concat(s2)`

-   用`+`连接字符串和其他数据类型，会将其他数据类型先自动转型为字符串，再连接（内部是转换为StringBuilder再append再转换为String）

>   `String s = "age is " + 23;`
>
>   想要把数字转化为字符串，可以使用空字符串相加：
>
>   `String s = "" + a + b + c;    // abc均为int型`
>
>   把int值视为字符的Unicode编码，然后将它们拼成一个字符串 ：
>
>   `String s = "" + (char)a + (char)b + (char)c;!`

**字符串的不可变特性**：字符串的内容不可变，但指向可以改变

```java
String s = "hello";
s = "world";		// s的指向改变，两个字符串在内存中拥有不同的地址
```

**判断两个字符串是否相等**：

```java
String s1 = "abc";		// 常量池
String s2 = new String("abc");		// 堆内存中
System.out.println(s1==s2);			// false 两个对象的地址值不一样
System.out.println(s1.equals(s2)); 	// true
//要注意s1不能为null：s1 != null && s1.equals(s2)
```

**判断是否是空字符串**（三种方法）：

```java
s1.isBlank()	// 字符串是否为空或长度为 0 或由空白符 (whitespace) 构成
s1,length() == 0
"".equals(s1)
```



#### 字符串常用函数

-   **获取字符串长度**

    >   `str.length()`        注意比数组长度多了括号，`length()`方法源码返回的是`value.length`字段，因为该字段是private

-   **String转换为int**

    >   `Integer.parseInt(str)`
    >
    >   `Integer.valueOf(str).intValue()`

-   **int转换为String**

    >   `String s = String.valueOf(num);`
    >
    >   `String s = Integer.toString(num);`
    >
    >   `String s = "" + num;`        相对于前两种耗时较大

-   **大小写转换**

    >   `str.toLowerCase()`        将字符串中的字母全部转换为小写，非字母不受影响
    >
    >   `str.toUpperCase()`        将字符串中的字母全部转换为大写，非字母不受影响

-   **去除字符串中的空格**

    >   `str.trim()`         去掉字符串中的首尾空格

-   **截取字符串**

    >   `str.substring(int beginIndex)`        从起始索引位置开始至结尾处
    >
    >   `str.substring(int beginIndex，int endIndex)`         从起始索引到结束索引（不包括结束索引）的字符串


-   **分割字符串**

    >   ` String[] arr = str.split(String sign) `         sign为指定的分割符，返回一个字符串数组
    >
    >   注意`"."`和`"|"`都是转义字符，如果以它们为分隔符，必须加上`"\\"`：` str.split("\\.") `

-   **字符串替换**

    >   `str.replace(String oldChar, String newChar)`         将字符串中所有oldChar替换成newChar
    >
    >   `str.replaceFirst(String regex, String replacement)`
    >
    >   `str.replaceAll(String regex, String replacement)`

-   **字符串比较**

    >   `str1.equals(str2)`        比较两个字符串是否具有相同的字符和长度，`==`则是用于比较是否是相同的字符串实例（即地址相同）
    >
    >   `str1.equalsIgnoreCase(str2)`        比较时不区分大小写
    >
    >   `str.compareTo(String otherstr)`        按字典顺序比较字符串str和otherstr，小于则返回负整数，相等则返回0，大于则返回正整数

-   **根据字符查找**

    >   ` str.indexOf(value, int fromIndex) `        返回`vaule`字符（串）在指定字符串str中**首次**出现的索引位置，如果能找到则返回索引值，否则返回 -1。`fromIndex` 表示查找时的起始索引，如果不指定 `fromIndex`，则默认从指定字符串中的开始位置（即 `fromIndex` 默认为 0）开始查找。
    >
    >   ` str.lastlndexOf(value, int fromIndex) `        返回`value`字符（串）在指定字符串str中**最后一次**出现的索引位置，如果能找到则返回索引值，否则返回 -1。**该方法是从右往左查找**，指定`fromIndex`时也是从该索引处往左查找。

-   **根据索引获取字符**

    >   `str.charAt(i)`         获取字符串的第i个字符 

-   **判断字符串是否以指定的前缀开始或后缀结束**

    >   `str.startsWith(String prefix)`
    >
    >   `str.endsWith(String suffix)`



#### 数组

```java
int[] arr = new int[5];		// 数组初始化，默认全0
int[] arr = new int[] { 1, 2, 3, 4, 5 };
int[] arr = { 1, 2, 3, 4, 5 };
int len = arr.length;		// 数组大小
```

使用`for each`循环遍历数组

```java
for (int item : ns) {
    ;
}
```

打印二维数组

```java
for (int[] arr : ns) {
    for (int n : arr) {
        System.out.print(n);
        System.out.print(', ');
    }
    System.out.println();
}
```

 或者使用Java标准库的`Arrays.deepToString(ns)`。



#### 格式化输出

 格式化输出使用`System.out.printf()`，通过使用占位符`%?`，**`printf()`**可以把后面的参数格式化成指定格式：`System.out.printf("%.2f\n", d);`

>   `print()`输出不换行，`println()`输出换行

| 占位符 |               说明               |
| :----: | :------------------------------: |
|   %d   |          格式化输出整数          |
|   %x   |      格式化输出十六进制整数      |
|   %f   |         格式化输出浮点数         |
|   %e   | 格式化输出科学计数法表示的浮点数 |
|   %s   |           格式化字符串           |

#### 输入

-   首先导入`java.util.Scanner`

-   然后创建`Scanner`对象并传入`System.in` 
-   使用`scanner.nextLine()`读取用户输入的**字符串**，使用`scanner.nextInt()` 读取用户输入的**整数**

>   依此类推，`scanner.nextByte()`、`scanner.nextShort()`、`scanner.nextLong()`、`scanner.nextFloat()`、`scanner.nextDouble()`、

```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); 	// 创建Scanner对象
        System.out.print("Input your name: ");	// 打印提示
        String name = scanner.nextLine(); 	// 读取一行输入并获取字符串
        System.out.print("Input your age: "); 	// 打印提示
        int age = scanner.nextInt(); 		// 读取一行输入并获取整数
        System.out.printf("Hi, %s, you are %d\n", name, age); 	// 格式化输出
        scanner.close();		// 一定要关闭
    }
}
```



#### 排序

冒泡排序：在里层的for循环里，`j`的取值范围是`arr.length-1-i`；注意是`j`和`j+1`比较(冒泡)

```java
public static void bubleSort(int[] ns, int n) {
    for (int i = 0; i < n - 1; ++i) {
        for (int j = 0; j < n - 1 - i; ++j) {
            if (ns[j] > ns[j + 1]) {
                int temp = ns[j];
                ns[j] = ns[j + 1];
                ns[j + 1] = temp;
            }
        }
    }
}
```

Java内置了排序方法

```java
int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
Arrays.sort(ns);		// 需要import java.util.Arrays;
System.out.println(Arrays.toString(ns));	// 排序后数组输出为字符串
```

>   Java内置排序方法使用的是`DualPivotQuicksort`，即双端快速排序（eclipse中选中函数按F3可快速查看源码）



## 面向对象基础

类（class）是一种抽象的概念 ，实例（instance）是具体的对象。

 使用`new`操作符创建一个实例，然后定义一个引用类型的变量来指向这个实例 ：

```java
Person ming = new Person();		// 左边定义变量，右边创建实例
```

```java
class Person {
	/*
	 * 把字段设置为私有类型是为了防止外部代码破坏封装性，但是需要额外定义方法(method)来访问字段和为字段赋值
	 * 外部代码可以调用方法setName()和setAge()来间接修改private字段；在方法内部，就有机会检查参数是否规范
	 */
	private String name;
	private Integer age;
    
	public String getName() {
        return this.name;		// this变量始终指向当前实例，只能在类定义的方法中使用
    }

    public int getAge() {
        return this.age;
    }
    
    public void setName(String name) {
    	if (name == null || name.isBlank()) {
            throw new IllegalArgumentException("invalid name");
        }
        this.name = name;
    }
    
    public void setAge(int age) {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("invalid age value");
        }
        this.age = age;
    }
```

#### 引用传递区别

>   [辨析](https://www.liaoxuefeng.com/wiki/1252599548343744/1260452774408320#0)：
>
>   1.  某个对象实例具有`String[] names`字段，传入一个字符串数组`fullnames`后，在外部修改`fullnames`中的某个元素，问该对象实例对应的`names`字段是否改变？
>   2.  另一个对象实例具有`String name`字段，传入一个字符串`name`后，修改`name`，问该对象实例对应的`name`字段是否改变？
>
>   答案：1改变；2不变
>
>   原因：1中`names`字段和`fullnames`指向的是同一个数组， 双方任意一方对这个数组的修改，都会影响对方，因为这两个变量共享同一块堆区 。2中修改`name`相当于将它指向另一个地址，对实例的`name`字段没有影响（仍指向原来的字符串），因为这两个变量分别指向不同的堆区。



#### 构造方法

构造方法的名称就是类名， 没有返回值（也没有`void`），调用构造方法必须用`new`操作符， 默认返回类型就是该类的实例。

>   每个类可以具有多个构造方法，但要求它们各自包含不同的方法参数（一般分为无参构造方法和有参构造方法两种 ）。 

```java
class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

>   如果自定义了构造方法， 那么编译器就不再自动创建默认构造方法
>
>   可以重载构造方法，以针对输入的不同的参数



#### 继承

Java使用`extends`关键字来实现继承 

```java
class Student extends Person {
    // 不要重复name和age字段/方法,
    // 只需要定义新增score字段/方法:
    private int score;

    public int getScore() { … }
    public void setScore(int score) { … }
}
```

>   注意：子类自动获得了父类的所有字段，严禁定义与父类重名的字段！ 

-   **protected关键字**

子类无法访问父类的`private`字段或者`private`方法，如果把`private`改为`protected`，就能让子类可以访问父类的字段

-   **super关键字**

`super`关键字表示父类，当子类引用父类的字段或方法时，可以用`super.fieldName`和`super.method()`（~~`this.fieldName`~~；this变量始终指向当前实例，虽然子类继承了父类的字段或方法，但在覆写时用this容易引起歧义）

由于子类**不会继承**任何父类的构造方法，在子类的构造方法中就必须调用父类的构造方法，可以使用`super(...)`，例如：

```java
class Student extends Person {
    protected int score;

    public Student(String name, int age, int score) {
        super(name, age); // 调用父类的构造方法Person(String, int)
        this.score = score;
    }
}
```

-   **向上转型**

例如`Person p = new Student()`这样的操作是允许的，即用父类的变量指向子类的实例。

这种把一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting）。

-   **向下转型**

如果把一个父类类型强制转型为子类类型，就是向下转型（downcasting）。

向下转型很可能会失败，失败的时候，Java虚拟机会报`ClassCastException`。

```java
Person p1 = new Student(); // upcasting, ok
Person p2 = new Person();
Student s1 = (Student) p1; // ok
Student s2 = (Student) p2; // runtime error! ClassCastException!
```

 为了避免向下转型出错，Java提供了`instanceof`操作符，可以先判断一个实例究竟是不是某种类型 ：

```java
Person p = new Person();
System.out.println(p instanceof Person); // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person); // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```

>   `instanceof`实际上是判断一个变量所**指向的实例**是否是指定类型，或者指定类型的子类 。
>
>   如果一个引用变量为`null`，那么对任何`instanceof`的判断都为`false` 

```java
Person p = new Student();
if (p instanceof Student) {
    // 只有判断成功才会向下转型:
    Student s = (Student) p; // 一定会成功
}
```



#### 多态

在继承关系中，子类如果定义了一个与父类方法名称、参数、返回值完全相同的方法，被称为覆写（Override）。

>   加上`@Override`可以让编译器帮助检查是否进行了正确的覆写。

若一个父类变量指向子类实例，并且子类覆写了父类的方法，父类变量调用该方法时，实际**执行的是子类的方法**。这个非常重要的特性在面向对象编程中称之为多态（Polymorphic）。

**多态是指，针对某个类型的方法调用，其真正执行的方法取决于运行时实际类型的方法。**

```java
public static double totalTax(Income... incomes) {
    double total = 0;
    for (Income income: incomes) {
        // Income派生出来的类重写getTax方法，实现不同的计算方法
        total = total + income.getTax();	
    }
    return total;
}
```

>   **覆写Object方法**：
>
>   因为所有的class都继承自Object类，而Object类有几个重要的方法：`toString()`、`equals()`
>
>   可以在自己定义的类中覆写这些方法
>
>   ```java
>   class Person {
>       ...
>       // 显示更有意义的字符串:
>       @Override
>       public String toString() {
>           return "Person:name=" + name;
>       }
>   
>       // 比较是否相等:
>       @Override
>       public boolean equals(Object o) {
>           // 当且仅当o为Person类型，并且name字段相同时，返回true
>           if (o instanceof Person) {
>               Person p = (Person) o;
>               return this.name.equals(p.name);
>           }
>           return false;
>       }
>   }
>   ```



#### final关键字

-   如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`。**用`final`修饰的方法不能被`Override`**。
-   如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为`final`。**用`final`修饰的类不能被继承**。
-   如果一个类的实例字段在初始化后不能被修改，可以把该字段标记为`final`。**用`final`修饰的字段或局部变量不能被重新赋值（即常量）**。一般在构造方法中初始化final字段。



#### 抽象类

如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么可以把父类的方法声明为抽象方法，同时必须把`Person`类本身也声明为`abstract`，才能正确编译它：

```java
abstract class Person {
    public abstract void run();
}
```

抽象类无法被实例化：`Person p = new Person()`会出现编译错误。

抽象类只能用于被继承，可以强迫子类实现其定义的抽象方法。

#### 接口

接口`interface`与抽象类的区别是不能定义实例字段，只有抽象方法，但可以定义`default`方法，让实现接口的各个类继承该方法。

```java
interface Person {
    String getName();
    default void run() {
        System.out.println(getName() + " run");
    }
}

class Student implements Person {	// 可以implements多个interface
    private String name;
	...
    public String getName() {
        return this.name;
    }
}
```



#### 静态字段和静态方法

用`static`修饰的字段称为静态字段，是被该类的所有实例共享的。修改某个实例的静态字段，所有实例的静态字段均会被修改。

>   推荐用类名来访问静态字段： `Person.number = 99`
>
>   同样可以通过类名调用静态方法：`Person.setNumber(99)`

静态方法内部，无法访问`this`变量，也无法访问实例字段，它只能访问静态字段。

>   接口不能定义实例字段，但可以有静态字段，并且必须定义为 `public static final`类型 。



#### package和import

Java内建的`package`机制是为了避免`class`命名冲突；

完整类名应该是`包名.类名`；多层包用`.`隔开（包名对应了文件的目录）；

一般在文件的第一行用`package xxx`来表示当前文件所属于的包；

第二行，可以使用`import`导入其他包里的类：`import mr.jun.Arrays;`或者`import mr.jun.*;`

JDK的核心类使用`java.lang`包，编译器会自动导入；

JDK的其它常用类定义在`java.util.*`，`java.math.*`，`java.text.*`，……；



#### classpath和jar

`classpath`是用来指示JVM如何搜索`class`的，一般是bin文件夹

不推荐在系统环境变量中设置`classpath`，在命令行运行程序时可传入`-cp`参数：

```bash
java -cp .;C:\work\project1\bin;C:\shared abc.xyz.Hello		// Windows用;分隔
java -cp /usr/shared:/usr/local/bin:/home/liaoxuefeng/bin	// Linux用:分隔
```

jar包可用来包含多个`.class`文件，变成一个zip格式的压缩文件。

jar包可以包含一个`MANIFEST.MF`文件，用于指定`Main-Class`等信息，用下面的命令可直接运行jar包。

```bash
java -jar xxx.jar
```

大型项目中可使用[maven](https://www.liaoxuefeng.com/wiki/1252599548343744/1255945359327200)非常方便地创建jar包。



#### 模块

引入模块是为了解决各class之间的依赖关系，模块以`.jmod`扩展名标识。

将一堆class封装为模块，不但需要打包，还需要在`src`目录下创建`module-info.java`文件，写入依赖关系：

```java
module hello.world {
    exports xxx;	// 其他模块想要使用本模块的类，需要将该类“导出”
    
	requires java.base; // 可不写，任何模块都会自动引入java.base
	requires java.xml;
    requires ...;
}
```

经过编译后会在bin目录中生成`module-info.class`文件，然后使用`jar`命令打包bin目录下的所有class文件，得到jar包

```bash
jar --create --file hello.jar --main-class com.itranswarp.sample.Main -C bin .
```

>   `hello.jar`（输出文件） 
>
>   `--main-class com.itranswarp.sample.Main`（Main定位） 
>
>   `-C bin .` (输入文件)

再使用`jmod`命令把jar包转换成模块，得到jmod文件（就是打包得到的模块）

```bash
jmod create --class-path hello.jar hello.jmod
```

可以用下面的命令**运行模块**：

```bash
java --module-path hello.jar --module hello.world
```

**打包JRE**:

将JRE的标准库拆分成模块，只需要带上用到的模块：

```bash
jlink --module-path hello.jmod --add-modules java.base,java.xml,hello.world --output jre/
```

>   `--module-path`参数指定模块`hello.jmod`
>
>   `--add-modules`参数指定用到的3个模块`java.base`、`java.xml`和`hello.world`，用`,`分隔
>
>   `--output`参数指定输出目录

运行JRE：

```bash
jre/bin/java --module hello.world
```





## Java核心类

#### StringBuilder

#### StringJoiner

#### JavaBean

#### 枚举

#### 常用工具类
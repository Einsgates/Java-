# Java基本功

## 入门

### 语言特性

- 面向对象

  封装、继承、多态

- 平台无关

  Java虚拟机实现平台无关化

### JVM之JDK与JRE

Java虚拟机是运行Java字节码的虚拟机，JVM有针对不同系统的特定实现，目的是使用相同的字节码，他们都会给出相同的结果。

- JDK

  Java Development Kit
  拥有JRE所拥有的一切，还有编译器(javac)和工具(javadoc和jdb).
  能够创建和编译工具

- JRE

  Java Runtime Environment
  Java运行时环境。它是运行已编译Java程序所需的所有内容的集合，包括Java虚拟机，Java类库，Java命令和其他一些基础构建

### Java程序执行过程

.java文件(源代码) 通过 [JDK中的javac编译]
.calss文件(JVM可以理解的Java字节) 通过 [JVM]
机器可执行的二进制机器码

## 语法

### 字符型常量与字符串常量

- 字符型常量

	- 形式

	  单引号引起的一个字符

	- 含义

	  相当于一个整型值 ASCII值，可以参加表达式运算

	- 内存大小

	  2个字节

- 字符串常量

	- 形式

	  双引号引起的0个或若干个字符

	- 含义

	  代表一个地址值(该字符串在内存中存放位置)

	- 内存大小

	  占若干字节

### 标识符与关键字

- 标识符

  一个名字

- 关键字

  被赋予特殊含义的标识符，常见：
  访问控制：private protected public
  类、方法和变量修饰符：abstract class extends final implements interface native new static strictfp synchronized volatile
  程序控制：break continue return do while if else for instanceof switch case default
  错误处理：try catch throw throws finally
  包相关：import package
  基本类型：boolean byte char double float int long short null true false
  变量引用：super this void
  保留字： goto const

### 泛型类型擦除与通配符

- 泛型

  generics是JDK 5引入的一个新特性，提供了编译时类型安全检测机制。
  泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数

	- 泛型类
	- 泛型接口
	- 泛型方法

- 类型擦除

  Java的泛型是伪泛型，因为Java在编译期间，所有的泛型类型都会被擦掉，也就是说的类型擦除

- 通配符

	- ? 表示不确定的Java类型
	- T (type) 表示具体的一个Java类型
	- K 代表Java键值中的key
	- V 代表Java键值中的value
	- E 代表element

### ==与equals区别

- ==

  判断两个对象的地址是否相等，即两个对象是否为同一个对象
  (基本数据类型：比较值；引用数据类型：比较内存地址)
  因为Java只有值传递，所以==本质比较的都是值，只是引用类型变量存的值是对象的地址

- equals

  判断两个对象是否相等，不能用于比较基本数据类型的变量
  equals()方法存在于Object类中，而Object类是所有类的直接或间接父类。
  equals存在两种使用情况：
  类没有覆盖equals()方法：等价于通过"=="比较这两个对象，使用默认的Object类的equals()方法
  类覆盖了equals方法。一般，都覆盖equals()方法来比较两个对象的内容相等；若内容相等，返回true
  注：
  String中的equals()方法是被重写过的，因为Object的equals方法比较的是内存地址，而String的equals方法比较的是对象的值
  当创建String类型的对象时，虚拟机会在常量池中查找有没有已经存在的值和要创建的值相同的对象，如果有就把它赋给当前的引用。没有就在常量池中创建一个String对象

### hashcode与equals

- hashCode()

  hashCode()作用获取哈希码，也称散列码。返回一个int数组，哈希码的作用是确定该对象在哈希表中的索引位置。

- 为什么重写equals必须重写hashCode方法

  如果两个对象相等，则hashCode一定也相同，对象调用equals方法都返回true。
  但是，两个对象hashCode相同，它们不一定相等。
  因此，equals方法被覆盖过，则hashCode也必须被覆盖。

## 基本数据类型

### 基本数据类型

6种数字类型：byte short int long float double
1种字符类型：char
1种布尔类型：boolean
基本类型		位数		字节		默认值
int				32			4			0
short 			16			2			0
long				64			8			0L	
byte				8				1			0
char				16			2		'u0000'
float				32			4			0f
double			64			8			0d
boolean		1						 false

### 自动装箱与拆箱

- 装箱

  将基本类型用它们对应的引用类型包装起来

- 拆箱

  将包装类型转换为基本数据类型

### 基本类型的包装类和常量池

Java基本类型的包装类的大部分都实现了常量池技术，即Byte Integer Long Character Boolean
前四种包装类默认创建了数值[-128, 127]的相应类型缓存数据，Character创建了数值在[0, 127]范围的缓存数据，Boolean直接返回true or false

## 方法

### Java只有值传递

按值调用：表示方法接收的是调用者提供的值
按引用调用：表示方法接收的是调用者提供的变量地址
一个方法可以修改传递引用所对应的变量值，而不能修改传递值调用所对应的变量值。
Java总是按值调用，也就是说，方法得到的是所有参数值的一个拷贝，即：方法不能修改传递给它的任何参数变量的内容
swap方法交换的是两个拷贝

一个方法不能修改一个基本数据类型的参数
一个方法可以改变一个对象参数的状态
一个方法不能让对象参数引用一个新的对象

### 重载与重写

- 重载

  同一个类中多个同名方法根据不同的传参来执行不同的逻辑处理

- 重写

  重写发生在运行期，是子类对父类的允许访问的方法的实现过程进行重新编写
  返回值类型、方法名、参数名必须相同，抛出异常范围<=父类，访问修饰符范围>=父类
  如果父类方法访问修饰符为private/final/static，则子类就不能重写该方法，但被static修饰的方法能够被再次声明
  构造方法无法被重写

- 区别

  区别点			重载方法		重写方法
  发生范围		同一个类		子类
  参数列表		必须修改		一定不能修改
  返回类型		可修改			子类方法返回值类型应<=父类
  异常				可修改			子类方法声明抛出的异常<=父类
  访问修饰符	可修改			不能做出严格的限制(可以限制降低)
  发生阶段		编译期			运行期

### 深拷贝 vs 浅拷贝

- 浅拷贝

  对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝

- 深拷贝

  对基本数据类型进行值传递，对引用数据类型，创建一个新的二对象，并复制其内容


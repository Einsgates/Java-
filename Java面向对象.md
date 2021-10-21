# Java面向对象

## 类和对象

### 面向对象与面向过程

- 面向过程

  面向过程比面向对象性能高，因为类的调用需要实例化，开销比较大，比较耗费资源，所以当性能是重要的考虑因素时，比如单片机、嵌入式开发、Linux/Unix等一般采用面向过程开发

- 面向对象

  面向对象易维护、易复用、易扩展。因为面向对象有封装、继承、多态等特性，所以可以设计出低耦合的系统，使系统更加灵活、更加易于维护。但是，相较于面向过程性能更低。

### 构造器相关

构造器Constructor能否被override?
Constructor不能被override(重写)，但可以overload(重载)，所以可以看到一个类中有多个构造函数的情况

### 成员变量与局部变量

- 语法形式

  成员变量属于类。
  局部变量是在代码块或方法中定义的变量或是方法的参数。
  成员变量可以被public private static等修饰符修数，而局部变量不行
  但是，二者皆可被final修饰

- 变量在内存中存储方式

  如果成员变量是使用static修饰的，那么这个成员变量是属于类的，若没有，则属于实例的。
  对象存在于堆内存，局部变量存在于栈内存

- 变量在内存中的生存时间

  成员变量是对象的一部分，它随着对象的创建而存在。
  而局部变量随着方法的调用而自动消失

- 赋值问题

  成员变量如果没有被赋值，则会以类型的默认值而赋值(一种情况例外：被final修饰的成员变量也必须显示地赋值)，而局部变量则不会被自动赋值。

### 对象实体与对象引用

用 new 创建对象实例(对象实例在堆内存中)，对象引用指向对象实例(对象引用存放在栈内存中)。
一个对象引用可以指向0个或1个对象；一个对象可以有n个引用指向它

### 构造方法

- 作用

  完成对类的初始化工作。
  如果一个类没有声明构造方法，该程序仍能够正确执行。Java会添加默认的无参数的构造方法。
  如果重载了有参数的构造方法，要把无参数的构造方法写出来，可以帮助我们在创建对象时少踩坑。

- 特性

  名字与类名相同
  没有返回值，但不能用void声明构造函数
  生成类的对象时自动执行，无需调用

- 父类子类

  在调用子类构造方法之前会先调用父类没有参数的构造方法，其目的是：帮助子类做初始化工作。

### 对象相等与其引用相等

- 对象相等

  比的是内存中存放的内容是否相等

- 其引用相等

  比的是指向的内存地址是否相等

## 面向对象三大特征

### 封装

封装是指把一个对象的状态信息(熟悉)隐藏在对象内部，不允许外部对象直接访问对象的内部消息。但可以提供一些被外界访问的方法来操作属性。

### 继承

继承是使用已存在的类的定义作为基础建立新的类的技术，新类的定义可以增加新的功能，也可以用父类的功能，但不能选择性地继承父类。

关于继承，有以下三点：
子类拥有父类所有的属性和方法(包括私有属性和私有方法)，但是父类中的私有属性和私有方法子类无法访问，只是拥有。
子类可以拥有自己的属性和方法，即子类可以对父类进行扩展。
子类可以用自己的方式实现父类的方法。

### 多态

- 定义

  一个对象具有多种状态，具体表现为父类的引用指向子类的实例。

- 特点

  对象类型和引用类型之间具有继承(类)/实现(接口)的关系
  对象类型不可变，引用类型可变
  方法具有多态性，属性不具有多态性
  引用类型变量发出的方法调用到底是哪个类中的方法，必须在程序运行期间才能确定
  多态不能调用“只在子类存在但在父类不存在”的方法
  如果子类重写了父类的方法，真正执行的是子类覆盖的方法，如果子类没有覆盖父类的方法，执行的是父类的方法

## 修饰符

### 静态方法与实例方法

在外部调用静态方法时，可以使用“类名.方法名”的方式，也可以使用“对象名.方法名"的方式。而实例方法只有后面这种方式，即：调用静态方法无需创建对象。
静态方法在访问本类的成员时，只允许访问静态成员(静态成员变量和静态方法)，而不允许访问实例成员变量和实例方法；实例方法则无此限制。

### 在静态方法能否调用非静态成员

不能。
由于静态方法可以不通过对象进行调用，因此在静态方法里，不能调用其他非静态变量，也不可以访问非静态变量成员。

## 接口和抽象类区别

### 常见区别

接口的方法默认是public，所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现)，而抽象类可以有非抽象方法。
接口中除了static final 变量，不能有其他变量，而抽象类中不一定。
一个类可以实现多个接口，但只能实现一个抽象类。接口自己本身可以通过 extends 关键字扩展多个接口
接口方法默认修饰符是 public，抽象方法可以有public protected 和 default 这些修饰符(抽象方法就是为了被重写，所以不能使用 private 关键字修饰！)
从设计层面上说，抽象是对类的抽象，是一种模板设计，而接口是对行为的抽象，是一种行为的规范。

### jdk7~jdk9接口概念变化

在jdk及早期版本中，接口里面只能有变量常量和抽象方法。这些接口方法必须由选择实现接口的类实现。
jdk8 接口可以有默认方法和静态功能方法
jdk 9 在接口中引入了私有方法和私有静态方法

## 其他

### 3种String类型

- String

  String 类中使用 final 关键字修饰字符数组来保存字符串，private final byte value[]
  所以，String对象是不可变的，

- StringBuffer

  继承自 AbstractStringBuilder 类。
  StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以说线程安全的。

- StringBuilder

  继承自 AbstractStringBuilder 类。
  线程不安全
  性能比 StringBuffer 稍高

- 三者总结

  操作少量数据：String
  单线程操作字符串缓冲区下操作大量数据：StringBuilder
  多线程操作字符串缓冲区下操作大量数据：StringBuffer

### Object类常见方法

public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。

public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。
public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。

protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。

public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。

public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。

public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。

public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。

public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。

public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念

protected void finalize() throws Throwable { }//实例被垃圾回收器回收的时候触发的操作

### == 与 equals

- ==

  判断两个对象的地址是否相等。
  基本数据类型：比较值
  引用数据类型：内存地址

- equals

  判断两个对象是否相等
  类没有覆盖 equals() 方法，则通过 equals() 比较该类的两个对象时，等价于通过 ”==“ 比较这两个对象
  类覆盖了 equals 方法。一般，我们都覆盖 equals() 方法来比较两个对象的内容是否相等。相等->true

### hashCode

- 介绍

  hashCode() 的作用是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。
  散列表中存储的是键值对(key-value)，它的特点是：根据”键“找出对应的”值“，这其中就利用了散列码。

- 为什么要有hashCode?

  我们先以“HashSet 如何检查重复”为例子来说明为什么要有 hashCode： 
  当你把对象加入 HashSet 时，HashSet 会先计算对象的         hashcode 值来判断对象加入的位置，同时也会与该位置其他已经加入的对象的 hashcode 值作比较，如果没有相符的 hashcode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashcode 值的对象，这时会调用 equals()方法来检查 hashcode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。（摘自我的 Java 启蒙书《Head first java》第二版）。这样我们就大大减少了 equals 的次数，相应就大大提高了执行速度。
  通过我们可以看出：hashCode() 的作用就是获取哈希码，也称为散列码；它实际上是返回一个 int 整数。这个哈希码的作用是确定该对象在哈希表中的索引位置。hashCode()在散列表中才有用，在其它情况下没用。在散列表中 hashCode() 的作用是获取对象的散列码，进而确定该对象在散列表中的位置。

- hashCode与equals相关规定

  如果两个对象相等，则 hashCode 一定也是相同的
  两个对象相等，对两个对象分别调用 equals 方法都返回 true
  两个对象有相同的 hashCode 值，它们不一定相等
  equals 方法被覆盖过，则 hashCode 方法也必须被覆盖
  hashCode() 的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等(即使这两个对象指向相同的数据)

### 序列化与 transient

对于不想序列化的变量，使用 transient 关键字修饰
transient 关键字作用：阻止实例中那些用此关键字修饰的变量序列化；当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复。transient 只能修饰变量，不能修饰类和方法。

### 获取用键盘输入常用的两种方法

- 通过 Scanner

  Scanner input = new Scanner(Ststem.in);
  String s = input.nextLine();
  input.close();

- 通过 BufferedReader

  BufferedReader input = new BufferedReader(new InputStreamerReader(System.in));
  String s = input.readLine();


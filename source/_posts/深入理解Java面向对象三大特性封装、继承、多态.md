---
title: 深入理解Java面向对象三大特性封装、继承、多态
date: 2019-03-08 14:34:52
tags:
    - java
categories:
    - java
---

## 1.封装

### 1.1 封装的定义

> 首先是抽象，把事物抽象成一个类，其次才是封装，将事物拥有的属性和动作隐藏起来，只保留特定的方法与外界联系,封装主要是因为Java有访问权限的控制。public>protected（继承访问权限）>package = default（包访问权限） > private。封装可以保护类中的信息，只提供想要被外界访问的信息。

Java中的访问权限控制主要分为四个级别，如下表

修饰符|当前类|同包|子类|其他包
:-:|:-:|:-:|:-:|:-:
private|√|×|×|×
dufalut|√|√|×|×
protect|√|√|√|×
public|√|√|√|√

### 1.2 为什么需要封装

> 封装符合面向对象设计原则的第一条：单一性原则，一个类把自己该做的事情封装起来，而不是暴露给其他类去处理，当内部的逻辑发生变化时，外部调用不用因此而修改，他们只调用开放的接口，而不用去关心内部的实现

### 1.3 实现

```java
public class Human
{
    private int age;
    private String name;

    public int getAge()
    {
        return age;
    }

    public void setAge( int age ) throws Exception
    {
        //封装age的检验逻辑，而不是暴露给每个调用者去处理
        if( age > 120 )
        {
            throw new Exception( "Invalid value of age" );
        }
        this.age = age;
    }

    public String getName()
    {
        return name;
    }

    public void setName( String name )
    {
        this.name = name;
    }
}
```

## 2. 继承

### 2.1 Java的类可以分为三类

- 类：使用class定义，没有抽象方法
- 抽象类：使用abstract class定义，可以有也可以没有抽象方法
- 接口：使用interface定义，只能有抽象方法

### 2.2 在这三个类型之间存在如下关系

- 类可以extends：类、抽象类（必须实现所有抽象方法），但只能extends一个，可以implements多个接口（必须实现所有接口方法）
- 抽象类可以extends：类，抽象类（可全部、部分、或者完全不实现父类抽象方法），可以implements多个接口（可全部、部分、或者完全不实现接口方法）
- 接口可以extends多个接口

### 2.3 继承以后子类可以得到什么

- 子类拥有父类非private的属性和方法
- 子类可以添加自己的方法和属性，即对父类进行扩展
- 子类可以重新定义父类的方法，即多态里面的覆盖，后面会详述

### 2.4 关于构造函数

- 构造函数不能被继承，子类可以通过super()显示调用父类的构造函数
- 创建子类时，编译器会自动调用父类的 无参构造函数
- 如果父类没有定义无参构造函数，子类必须在构造函数的第一行代码使用super()显示调用

类默认拥有无参构造函数，如果定义了其他有参构造函数，则无参函数失效，所以父类没有定义无参构造函数，不是指父类没有写无参构造函数。看下面的例子，父类为Human，子类为Programmer。

```java
public class Human
{
    //定义了有参构造函数，默认无参构造函数失效
    public Human(String name)
    {
    }
}
```

```java
public class Programmer
    extends Human
{
    public Programmer()
    {
        //如不显示调用，编译器会出现如下错误
        //Implicit super constructor Human() is undefined. Must explicitly invoke another constructor
        super( "x" );
    }
}
```

### 2.5 为什么需要继承

> 代码重用是一点，最重要的还是所谓想上转型，即父类的引用变量可以指向子类对象，这是Java面向对象最重要特性多态的基础

## 3. 多态

### 3.1 在了解多态之前，首先需要知道方法的唯一性标识即什么是相同/不同的方法

- 一个方法可以由：修饰符如public、static+返回值+方法名+参数+throw的异常5部分构成
- 其中只有方法名和参数是唯一性标识，意即只要方法名和参数相同那他们就是相同的方法
- 所谓参数相同，是指参数的个数，类型，顺序一致，其中任何一项不同都是不同的方法

### 3.2 何谓重载

- 重载是指一个类里面（包括父类的方法）存在方法名相同，但是参数不一样的方法，参数不一样可以是不同的参数个数、类型或顺序
- 如果仅仅是修饰符、返回值、throw的异常 不同，那这是2个相同的方法，编译都通不过，更不要说重载了

```java
//重载的例子
public class Programmer
    extends Human
{
    public void coding() throws Exception
    {

    }

    public void coding( String langType )
    {

    }

    public String coding( String langType, String project )
    {
        return "";
    }
}
```

```java
//这不是重载，而是三个相同的方法，编译报错
public class Programmer
    extends Human
{
    public void coding() throws Exception
    {

    }

    public void coding()
    {

    }

    public String coding()
    {
        return "";
    }
}
```

### 3.3 何谓覆盖/重写

- 覆盖描述存在继承关系时子类的一种行为
- 子类中存在和父类相同的方法即为覆盖，何谓相同方法请牢记前面的描述，方法名和参数相同，包括参数个数、类型、顺序

```java
public class Human
{
    public void coding( String langType )
    {

    }
}
```

```java
public class Programmer
    extends Human
{
    //此方法为覆盖/重写
    public void coding( String langType )
    {

    }

    //此方法为上面方法的重载
    public void coding( String langType, String project )
    {

    }
}
```

### 3.4 覆盖/重写的规则

- 子类不能覆盖父类private的方法，private对子类不可见，如果子类定义了一个和父类private方法相同的方法，实为新增方法
- 重写方法的修饰符一定要大于或等于被重写方法的修饰符(public > protected > default > private)
- 重写抛出的异常需与父类相同或是父类异常的子类，或者重写方法干脆不写throws
- 重写方法的返回值必须与被重写方法一致，否则编译报错
- 静态方法不能被重写为非静态方法，否则编译出错

### 3.5 理解了上述知识点，就可以定义多态了

- 多态可以说是“一个接口，多种实现”或者说是父类的引用变量可以指向子类的实例，被引用对象的类型决定调用谁的方法，但这个方法必须在父类中定义
- 多态可以分为两种类型：编译时多态（方法的重载）和运行时多态（继承时方法的重写），编译时多态很好理解，后述内容针对运行时多态
- 运行时多态依赖于继承、重写和向上转型

```java
class Human
{
    public void showName()
    {
        System.out.println( "I am Human" );
    }
}

//继承关系
class Doctor
    extends Human
{
    //方法重写
    public void showName()
    {
        System.out.println( "I am Doctor" );
    }
}

class Programmer
    extends Human
{
    public void showName()
    {
        System.out.println( "I am Programmer" );
    }
}

public class Test
{
    //向上转型
    public Human humanFactory( String humanType )
    {
        if( "doctor".equals( humanType ) )
        {
            return new Doctor();
        }
        if( "programmer".equals( humanType ) )
        {
            return new Programmer();
        }
        return new Human();
    }

    public static void main( String args[] )
    {
        Test test = new Test();
        Human human = test.humanFactory( "doctor" );
        human.showName();//Output:I am Doctor
        human = test.humanFactory( "programmer" );
        human.showName();//Output:I am Programmer
        //一个接口的方法，表现出不同的形态，意即为多态也
    }
}
```

### 3.6 向上转型的缺憾

> 只能调用父类中定义的属性和方法，对于子类中的方法和属性它就望尘莫及了，必须强转成子类类型

### 3.7 总结概括

- 当超类对象引用变量引用子类对象时，被引用对象的类型而不是引用变量的类型决定了调用谁的成员方法，但是这个被调用的方法必须是在超类中定义过的，也就是说被子类覆盖的方法，但是它仍然要根据继承链中方法调用的优先级来确认方法，该优先级为：this.show(O)、super.show(O)、this.show((super)O)、super.show((super)O)

```java
class Human {

    public void fun1() {
        System.out.println("Human fun1");
        fun2();
    }

    public void fun2() {
        System.out.println("Human fun2");
    }
}

class Programmer extends Human {

    public void fun1(String name) {
        System.out.println("Programmer's fun1");
    }

    public void fun2() {
        System.out.println("Programmer's fun2");
    }
}

public class Test {
    public static void main(String[] args) {
        Human human = new Programmer();
        human.fun1();
    }
    /*
     * Output:
     *  Human fun1
     *  Programmer's fun2
     */
}
```

- Programmer中的fun1(String name) 和Human中的fun1()，只是方法名相同但参数不一样，所以是重载关系
- Programmer中的fun2()和Human中的fun2()是相同的方法，即Programmer重写了Human的fun2()方法
- 把Programmer的对象向上转型赋值个Human后，human.fun1()会调用父类中的fun1()方法，子类的fun1(String name)是不同的方法
- 在human的fun1()中又调用了fun2()方法，该方法在Programmer中被重写，实际调用的是被引用变量Programmer中的fun2()方法

```java
package test;

class A {
    public void func() {
        System.out.println("func in A");
    }
}

class B extends A {
    public void func() {
        System.out.println("func in B");
    }
}

class C extends B {
    public void func() {
        System.out.println("func in B");
    }
}

public class Bar {
    public void test(A a) {
        a.func();
        System.out.println("test A in Bar");
    }

    public void test(C c) {
        c.func();
        System.out.println("test C in Bar");
    }

    public static void main(String[] args) {
        Bar bar = new Bar();
        A a = new A();
        B b = new B();
        C c = new C();
        bar.test(a);
        bar.test(b);
        bar.test(c);
        /*
            func in A
            test A in Bar
            func in B
            test A in Bar
            func in B
            test C in Bar
         */
    }
}
```

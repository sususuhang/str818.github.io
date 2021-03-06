---
layout: article
title: Java 面向对象
tags: Java

lang: zh-Hans
key: Java_Object_Oriented
pageview: true
toc: true
show_subscribe: false
---

### 面向对象三大特征

1.封装

隐藏对象的属性和实现细节，仅对外提供公共访问方式。隐藏代码的实现细节，提高安全性。

2.继承

体现在代码开发中，让代码具有层次结构。继承的主要特点是父类与子类的关系，子类可以继承父类的一些特性，如方法和变量。

3.多态

主要体现在 Java 的重载与重写上。

### 访问控制权限

|     修饰词     | 本类 | 同一个包的类 | 子类 | 任何地方 |
| :------------: | :--: | :----------: | :--: | :------: |
|    private     |  √   |      ×       |  ×   |    ×     |
| default (默认) |  √   |      √       |  ×   |    ×     |
|   protected    |  √   |      √       |  √   |    ×     |
|     public     |  √   |      √       |  √   |    √     |

### 重写

在父类中方法无法满足子类需求时，子类可以将父类的方法进行重写来满足需求。

方法重写条件：

- 两个类必须是继承关系。
- 必须具有相同的方法名，相同的返回值类型，相同的参数列表。
- 子类的访问权限必须大于等于父类方法。
- 子类方法的返回类型必须是父类方法返回类型或为其子类型。

私有方法、静态方法、构造方法不能被重写。

### super

当前子类父类型的特征。

- 访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。
- 访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。

### 抽象类

抽象类和抽象方法都使用 abstract 关键字进行声明，且不能被 final 修饰。抽象类一般会包含抽象方法，抽象方法一定位于抽象类中。抽象类中的子类可以是抽象类，如果不是抽象类的话必须对抽象类中的方法进行重写。

### 接口

接口是抽象类的延伸，在 Java 8 之前，它可以看成是一个完全抽象的类，也就是说它不能有任何的方法实现。

从 Java 8 开始，接口也可以拥有默认的方法实现，这是因为不支持默认方法的接口的维护成本太高了。在 Java 8 之前，如果一个接口想要添加新的方法，那么要修改所有实现了该接口的类。

一个类如果实现了接口，那么这个类需要重写接口中所有的抽象方法，如果不重写则这个类需要声明为抽象类。

接口可以使项目分层，都面向接口开发，提高开发效率，同时也降低了代码之间的耦合度，提高了代码的可插拔性。

接口的成员（字段 + 方法）默认都是 public 的，并且不允许定义为 private 或者 protected。

接口的字段默认都是 static 和 final 的。

### 内部类

内部类可以直接访问外部类的成员，包括 private 修饰的变量和方法。

- 静态内部类
- 匿名内部类
- 成员内部类
- 局部内部类

### Object

#### 1. toString()

用来被子类重写的，该方法将返回此对象的字符串表示，开发自己的类如果没有特殊要求都应该重写 toString 方法。

#### 2. equals()

让继承 Object 的类重写，以满足比较不同类型对象是否等价的要求。String 已经对 equals 方法进行了重写。

```java
public boolean equals(Object obj){
  return (this == obj);
}
```

##### 重写规则

Ⅰ 自反性

```java
x.equals(x); // true
```

Ⅱ 对称性

```java
x.equals(y) == y.equals(x); // true
```

Ⅲ 传递性

```java
if (x.equals(y) && y.equals(z))
    x.equals(z); // true;
```

Ⅳ 一致性

多次调用 equals() 方法结果不变

```java
x.equals(y) == x.equals(y); // true
```

Ⅴ 与 null 的比较

对任何不是 null 的对象 x 调用 x.equals(null) 结果都为 false

```java
x.equals(null); // false;
```

##### 实现

- 检查是否为同一个对象的引用，如果是直接返回 true
- 检查是否是同一个类型，如果不是，直接返回 false
- 将 Object 对象进行转型
- 判断每个关键域是否相等

```java
public class EqualExample {

    private int x;
    private int y;
    private int z;

    public EqualExample(int x, int y, int z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;

        EqualExample that = (EqualExample) o;

        if (x != that.x) return false;
        if (y != that.y) return false;
        return z == that.z;
    }
}
```

#### 3. hashCode()

hashCode() 返回散列值，而 equals() 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价。

在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等。

#### 4. finalize()

垃圾回收之前，自动调用此方法。

### 关键字

#### static

##### 1. 静态变量

程序运行时静态变量存放在方法区里面，因此，静态变量在类加载阶段赋值，并且只赋值一次。

##### 2. 静态方法

不用创建对象就能直接方法该方法，即使用「类名.静态方法名」的方式。静态方法不能访问非静态的数据。

##### 3. 静态语句块

静态语句块在类加载阶段执行，只执行一次，按照自上而下的顺序执行，在构造方法之前执行。

##### 4. 静态内部类

非静态内部类依赖于外部类的实例，而静态内部类不需要。

#### final

final 修饰的基本数据类型，值不能改变；final 修饰的引用数据类型，指向地址不能改变，但是对象里面的值是可以改变的。

- **类** — 无法被继承。
- **方法** — 无法被重写。
- **局部变量** — 一旦赋值，不可再改变。
- **成员变量** — 必须初始化值。

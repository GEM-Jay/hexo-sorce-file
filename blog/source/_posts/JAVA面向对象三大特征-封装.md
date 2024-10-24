---
title: JAVA面向对象三大特征--封装
tags:
  - 技术
categories:
  - 技术
repo: GEM-Jay/GEM-Jay.github.io
abbrlink: 1511018263
date: 2024-10-16 13:16:16
---

# JAVA面向对象三大特征—封装

封装是面向对象编程中的三大特征之一，在对封装性进行解释时我们有必要先了解一些面向对象的思想，以及相关的概念。当我们想要去描述一系列的关系时我们要用到的最基本结构就是类，其中存在着成员变量和方法，用于记录属性和表达行为。

## 一、名词解读

为了解释封装的概念和作用，需要先来了解一下几个相关的概念，这有助于我们接下来的理解。

### 1.权限修饰符

当我们在一个类中定义成员变量时，会指定一个变量的类型，除此之外，还会有修饰符的部分，在此给出定义成员变量的规范格式：

```none
// 定义变量
[修饰符] 变量类型 变量名称;
[修饰符] 变量类型 变量名称 = 初始值;
```

修饰符起到的作用从字面就可以解释，起到一个修饰和限定的作用，可以使用在成员变量之前的修饰符可以是：public、protected、private、final、static。  
修饰符与修饰符之间的顺序没有强制要求，其中public、protected、private被称为权限修饰符，可以用来限定类的属性和方法的访问权限，指明在哪些包的哪些类中能够调用到这些属性或方法，是一种一定会存在的修饰符。需要注意的是，这三个单词不能同时出现，当这三个单词都不出现的时候会被认为是默认访问权限，所以权限修饰符一共有四种：private、默认、protected、public。

### 2.权限对应关系表

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly93d3cubWFya2VkaXRvci5jb20vZmlsZS9nZXQvYWVlODJiNjkxMTE2Njk5YzAyZDczOWNiOGFjOTg3NWQucG5n?x-oss-process=image/format,png)

* private：私有权限，只能在定义的类中访问，在其他类中创建的实例均无法访问
* 默认：同包可访问权限，在没有声明权限修饰符时为默认权限，允许在同包的其他类访问
* protected：受保护权限，允许有继承关系的子类访问
* public：公共权限，允许任何类访问

### 3.属性访问

由于权限修饰符在封装特性中的作用只是实现封装性的一种途径，所以在这里只演示private与public的作用，权限修饰符的其他作用将在后续的文章中继续介绍。

```none
src
└──edu
    └──sandtower
        └──bean
            │    Person.java
        └──test
            │    Test.java
```

以上为实体类与测试类所在的目录结构，Person实体类所在包：edu.sandtower.bean，Test测试类所在包：edu.sandtower.test，相应代码如下：

```none
package edu.sandtower.bean;

public class Person{
    // 声明公开属性
    public String name;
    // 声明私有属性
    private double money;

}
```

```none
package edu.sandtower.test;

import edu.sandtower.bean.Person;

public class Test{
    public static void main(String[] args){
        // 在test包中的Test类中创建Person实例
        Person person = new Person();
        person.name = "小张";// 编译通过，可以访问name属性
        person.money = 500.0;// 编译失败，无法访问money属性
    }
}
```

从上面的例子可以看出，虽然依然是使用Person自己的实例在进行属性的调用，但是我们是在另外一个包中的类发生的调用，所以是不能够访问到private修饰的属性的，在刚开始学习时一定要注意区分。

## 二、概念阐释

### 1.封装有什么用

通过使用权限修饰符，我们可以限定类的成员的被访问权限，那为什么要这样做呢？在很多场景下，我们需要确保我们对属性值的操作均是有效操作，不能违背某些规则。  
比如，我们定义了一个Person类，具有name和money两个属性，在买东西时需要扣掉相应的金额，原始写法如下：

```none
public class Person{
    public String name;
    public double money;
}
```

```none
public class Test{
    public static void main(String[] args){
        Person person = new Person();
        person.money = 500;// 初始金额500元
        System.out.println("购买一张桌子，花费200元");
        person.money -= 200;
        System.out.println("购买二手PSP，花费350元");
        person.money -= 350;
        System.out.println("目前余额为：" + person.money);// -50
    }
}
```

可以看到，经过代码操作以后可能会导致money的属性为负。看官甲：你自己不加判断赖代码？没错，这个问题我们可以增加判断代码来解决，由于这个操作是对money属性值的操作，我们将它封装成一个方法写在实体类中，于是有了改进之后的代码：

class Person\{

 `public String name;
    public double money;

    // 定义一个方法，用于设置money属性的值
    public void setMoney(double money){
        if(money >= 0){
            this.money = money;
        }
    }
}```

```public class Test{
    public static void main(String[] args){
        Person person = new Person();
        person.money = 500;// 初始金额500元
        System.out.println("购买一张桌子，花费200元");
        person.setMoney(person.money - 200);
        System.out.println("购买二手PSP，花费350元");
        person.setMoney(person.money - 350);
        System.out.println("目前余额为：" + person.money);// 300
    }
}```

经过上面的改进，我们可以确保money的值不为负数，同时可以看到，当在实体类中定义方法后，使用者需要修改属性值时直接调用方法就可以保证不出问题。但是由于属性值依然可以被直接访问，还不能保证万无一失，于是我们利用权限修饰符使得变量不能被直接访问，同时需要定义一个能够取得属性值的方法。`

public class Person\{  
public String name;  
// 声明money属性为private权限  
private double money;

```
// 定义一个方法，用于设置money属性的值
public void setMoney(double money){
    if(money >= 0){
        this.money = money;
    }
}
// 定义一个方法，用于获取money属性的值
public double getMoney(){
    return this.money;
}
```

\}

```none
```
public class Test{
    public static void main(String[] args){
        Person person = new Person();
        person.setMoney(500);// 初始金额500元，此时已经不能使用对象.属性的方法赋值
        System.out.println("购买一张桌子，花费200元");
        person.setMoney(person.getMoney() - 200);
        System.out.println("购买二手PSP，花费350元");
        person.setMoney(person.getMoney() - 300);
        System.out.println("目前余额为：" + person.getMoney());// 300
    }
}
```

通过以上的案例，我们可以看到进行封装有以下几个作用：

防止类的属性被外部代码随意的修改和访问，保证数据的完备性  
将对属性的操作转换为方法，更加灵活和安全  
使用封装可以隐藏实现的细节：使用者只需要作用，不需要知道过程  
在类的定义结构中修改，提高了代码的可维护性，同时又可以不影响外部的使用  
通过封装方法可以有效减少耦合  
耦合：模块与模块之间，代码与代码之间的关联程度，对属性封装后，和调用相关的代码就会变得相对简单，可以降低耦合

### 2.如何进行封装

在进行封装时都是出于对属性保护的考虑，可以按照以下两个步骤来进行：

使用权限修饰符  
使用private作用在属性上，关闭直接访问的入口  
使用public作用在方法上，提供调用的入口  
定义与属性存取相关的方法  
在属性关闭后，我们需要通过方法来获取属性的值以及对属性值进行修改。由于有了方法结构，我们就可以对存入的数据进行判断，对不符合逻辑的数据进行处理。

### 3.常规封装方法

明白了封装的作用后，我们可以通过自定义方法的方式完成对属性的封装。封装方法和类中定义的其他方法在结构上没有任何的区别，同样都是普通的方法，区别主要在于体现在用途方面：

普通方法主要表达该类所能产生的行为  
封装方法主要为属性的访问和使用提供了一个入口，作用相对单一  
在进入到框架的学习之后，很多对实体类属性自动赋值的操作都是通过调用封装方法实现的，所以我们必须要知道常规封装方法的名称定义和类型设置规则。  
对于属性来说我们只会进行两种操作：存和取。那么相应的封装方法应该有一对儿

get代表取用：既然是取值，那么就要把属性值进行返回，方法的返回值类型与属性类型相同  
set代表存储：既然是存值，那么就要在参数列表中接收想要存入的值，类型与属性类型相同  
对于命名方面只要遵从驼峰命名法就好，以get或set开头，大写属性名称的首字母，其余不变，看下面一个例子：

```none
public class Person{
    // 使用private声明属性
    private String name;
    private double money;

    // 使用public声明方法，作为操作属性的入口
    public void setName(String name){
        this.name = name;
    }
    public String getName(){
        return this.name;
    }
    public void setMoney(double money){
        // 如有需要，可以在方法中可以自定义其他逻辑
        this.money = money;
    }
    public double getMoney(){
        return this.money;
    }
}
```

由于常规封装方法定义的格式和名称都相对固定，所以一般的编译器都自带自动生成封装方法的功能，这样既方便又能降低出错率，大家一定要掌握。

* Eclipse：

属性定义完成后，选择source菜单 \-> Generate Getters and Setters…

![](https://img-blog.csdnimg.cn/2020032520360256.png)

点击Select All（选择所有属性） \-> Generate

![](https://img-blog.csdnimg.cn/20200325203631845.png)
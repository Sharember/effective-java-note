# 3:用私有构造器或者枚举类型强化Singleton属性

## 1.简介

Singleton 指仅仅被实例化一次的类（单例）。Singleton 通常被用来代表那些本质上唯一的系统组件，比如窗口管理器或者文件系统。

## 2.旧式实现方式

在 JDK1.5 发行之前，实现 Singleton 有两种方法：

方法一：

```java
class Singletonone {
    public static final Singletonone SINGLETONONE = new Singletonone();
    private Singletonone(){};
    
}
```

利用构造器仅被调用一次，用来实例化公有的静态 final 域 Singletonone.SINGLETONONE;可以保证该类对象的全局唯一性。

> 这种写法可以通过反射来生成第二个实例，如果想要抵御这种攻击，可以修改构造器，在创建第二个对象的时候抛出异常。

方法二：

```java
class Singletontwo {
    private static final Singletontwo SINGLETONTWO = new Singletontwo();
    private Singletontwo(){};
    
    public Singletontwo getInstance(){
        return SINGLETONTWO;
    }

}
```

> 这种叫恶汉式，还有一种懒汉式的方式。但是多线程会有问题。

## 3.新的方式

jdk1.5 以后加入了枚举，我们可以通过枚举来实现。

```java
enum Singletonthree {
    SINGLETONTHREE
}
```

写完了。
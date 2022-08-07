# 一、模板方法设计模式

### 1.AQS队列

AQS的设计是使用**模板方法设计模式**，它将一些方法开放给子类进行重写，而同步器给同步组件所提供模板方法又会重新调用被子类所重写的方法。举个例子，AQS中需要重写的方法tryAcquire:

```Java
protected boolean tryAcquire(int arg){
    throw new UnsupportedOperationException();
}
```

ReentrantLock中NonfairSync（ReentrantLock的一个静态内部类，继承了Sync，Sync继承了AQS）会重写该方法为：

```Java
protected final boolean tryAcquire(int acquires) {
    return nonfairTryAcquire(acquires);
}
```

# 二、单例模式

### 1.懒汉模式-双重检验锁定

使用volatile和synchronized实现单例模式，使用双重检验锁定(double checked locking pattern)。

> 注意：一定要加上 volatile

```java
public class Singleton {
    private Singleton() { }
    private volatile static Singleton instance;
    public Singleton getInstance(){
        if(instance==null){
            synchronized (Singleton.class){
                if(instance==null){
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

为何要加上 volatile？  ------------>    具体原因是因为指令重排，见《java并发编程.md》的 ”有序性“部分内容。

### 2.懒汉模式-synchronized

```java
private static final Singleton instance;
private Singleton(){}

public static synchronized Singleton getInstance() {
    
    if (instance == null) {
        instance = new Singleton();
    }
    return instance;
}
```

### 3.饿汉模式

通过 static 和 final修饰变量，单例会在类加载后一开始就被初始化。

```java
public class Singleton{
    // 类加载时就初始化
    private static final Singleton instance = new Singleton();
    private Singleton(){}
    public static Singleton getInstance(){
        return instance;
    }
}
```

### 4.懒汉模式-静态内部类

-- 内部类加载机制

使用静态内部类，这种方法也是《Effective Java》上所推荐的，由于 SingletonHolder 是私有的，除了 getInstance() 之外没有办法访问它，因此它是懒汉式的；同时读取实例的时候不会进行同步，没有性能缺陷；也不依赖 JDK 版本。在getInstance()之前，是不会初始化内部类的中的静态变量值的。

```java
public class Singleton {  
    private static class SingletonHolder {  
        private static final Singleton INSTANCE = new Singleton();  
    }  
    private Singleton (){}  
    public static final Singleton getInstance() {  
        return SingletonHolder.INSTANCE; 
    }  
}
```


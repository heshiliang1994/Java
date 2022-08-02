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


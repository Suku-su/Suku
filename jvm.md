# Jvm

# 类加载器的生命周期



![image-20250815101856858](jvm.assets/image-20250815101856858.png)



![image-20250815103007878](jvm.assets/image-20250815103007878.png)



![image-20250815103015940](jvm.assets/image-20250815103015940.png)



![image-20250815103027274](jvm.assets/image-20250815103027274.png)





## 连接阶段

![image-20250815103219104](jvm.assets/image-20250815103219104.png)





![image-20250815104005385](jvm.assets/image-20250815104005385.png)

![image-20250815104932378](jvm.assets/image-20250815104932378.png)


在连接阶段

会给变量赋予初始值，

![image-20250815105022748](jvm.assets/image-20250815105022748.png)

==但是如果添加了final==关键字的话就可以直接把值给到他



解析阶段就是将符号应用转换为内存的直接引用



## 初始化

![image-20250815105336847](jvm.assets/image-20250815105336847.png)

静态代码块以及静态变量的顺序

![image-20250815105659002](jvm.assets/image-20250815105659002.png)

初始化对象的四种不同的方式

![image-20250815105926836](jvm.assets/image-20250815105926836.png)

当你需要使用一个类的静态变量，但是这个类的静态变量使用了final来修饰的时候，他是不会触发初始化的

![image-20250815110209589](jvm.assets/image-20250815110209589.png)



![image-20250815110853089](jvm.assets/image-20250815110853089.png)





![image-20250815111326107](jvm.assets/image-20250815111326107.png)



![image-20250815111500711](jvm.assets/image-20250815111500711.png)



![image-20250815111512732](jvm.assets/image-20250815111512732.png)

## 总结

![image-20250815111701411](jvm.assets/image-20250815111701411.png)

![image-20250815111711495](jvm.assets/image-20250815111711495.png)

# 类加载器

![image-20250815111918970](jvm.assets/image-20250815111918970.png)

应用

![image-20250815112047191](jvm.assets/image-20250815112047191.png)

路线

![image-20250815112100324](jvm.assets/image-20250815112100324.png)

## 类加载器的分类

![image-20250815112306191](jvm.assets/image-20250815112306191.png)

jdk8以及之前的类加载器

![image-20250815112730096](jvm.assets/image-20250815112730096.png)

## 启动类加载器

![image-20250815113459910](jvm.assets/image-20250815113459910.png)

加载java.lib下的jar

## 扩展

![image-20250815113944070](jvm.assets/image-20250815113944070.png)

#### 加载其他的jar

![image-20250815114433067](jvm.assets/image-20250815114433067.png)

# 双亲委派机制

作用

![image-20250815121645389](jvm.assets/image-20250815121645389.png)

机制

![image-20250815121817509](jvm.assets/image-20250815121817509.png)



常见考题

![image-20250815122239006](jvm.assets/image-20250815122239006.png)

怎么使用一个确定的类加载器去加载一个类

![image-20250815122348430](jvm.assets/image-20250815122348430.png)

双亲委派机制的继承与父类加载器

![image-20250815122714595](jvm.assets/image-20250815122714595.png)



面试题，类的双亲委派机制到底是什么

![image-20250815122925378](jvm.assets/image-20250815122925378.png)

# 如何打破双亲委派机制

![image-20250816093742125](jvm.assets/image-20250816093742125.png)

## tomcat类加载器

![image-20250816093931133](jvm.assets/image-20250816093931133.png)



![image-20250816094038547](jvm.assets/image-20250816094038547.png)
打破

![image-20250816094259243](jvm.assets/image-20250816094259243.png)

![image-20250816101851905](jvm.assets/image-20250816101851905.png)

![image-20250816102003286](jvm.assets/image-20250816102003286.png)

# SPI机制

service provider intereface

一种基于classloader 来发现并加载服务的机制

一个标准的spi，有3个组件

- service

  接口

- serviceprovider

- 实现类

  - serviceloader

  核心组件、负责在运行的时候发现并加载ServiceProvider

- ![bfbd136fcc70bc6851b088ad581f407d](jvm.assets/bfbd136fcc70bc6851b088ad581f407d.jpg)



![a053580cbe1b600c756eaed510450bd3](jvm.assets/a053580cbe1b600c756eaed510450bd3.jpg)![e18abefd8c32c998a6bea3a0437e4983](jvm.assets/e18abefd8c32c998a6bea3a0437e4983.jpg)![c34610b6ee0c59e4e347c3c948072b16](jvm.assets/c34610b6ee0c59e4e347c3c948072b16.jpg)![4b752c20035c2791f01684649519363d](jvm.assets/4b752c20035c2791f01684649519363d.jpg)





![image-20250816110529905](jvm.assets/image-20250816110529905.png)





JDBC

![image-20250816110806900](jvm.assets/image-20250816110806900.png)





![image-20250816111154156](jvm.assets/image-20250816111154156.png)

# jdk9以后的类加载器

**![image-20250816112252036](jvm.assets/image-20250816112252036.png)**

**启动类加载器使用java来编写，但是你还是拿不到，保障安全**

![image-20250816112450860](jvm.assets/image-20250816112450860.png)

## 总结

![image-20250816112544681](jvm.assets/image-20250816112544681.png)



![image-20250816112632700](jvm.assets/image-20250816112632700.png)



![image-20250816112656366](jvm.assets/image-20250816112656366.png)



![image-20250816112824493](jvm.assets/image-20250816112824493.png)

# 运行时数据区

![image-20250816113301462](jvm.assets/image-20250816113301462.png)

## 面试题

![image-20250816113414667](jvm.assets/image-20250816113414667.png)

## 应用场景

![image-20250816113441209](jvm.assets/image-20250816113441209.png)

## 路线

![image-20250816113456705](jvm.assets/image-20250816113456705.png)

# 程序计数器

![image-20250816113703381](jvm.assets/image-20250816113703381.png)



![image-20250816114228517](jvm.assets/image-20250816114228517.png)

# 栈



![image-20250816124456011](jvm.assets/image-20250816124456011.png)

![image-20250816124546135](jvm.assets/image-20250816124546135.png)

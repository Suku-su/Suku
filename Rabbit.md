# Rabbit

开始

![屏幕截图 2025-07-23 082912](Rabbit.assets/屏幕截图 2025-07-23 082912.png)



## 学习路线

![屏幕截图 2025-07-23 083005](Rabbit.assets/屏幕截图 2025-07-23 083005.png)

## 同步调用

![image-20250723083720236](Rabbit.assets/image-20250723083720236.png)

## 异步调用

![image-20250723084041581](Rabbit.assets/image-20250723084041581.png)

## 异步调用的优缺点

![image-20250723084359503](Rabbit.assets/image-20250723084359503.png)

![image-20250723084801905](Rabbit.assets/image-20250723084801905.png)



---

## 选型对比

![image-20250723085428267](Rabbit.assets/image-20250723085428267.png)

## 模型

![image-20250723101947159](Rabbit.assets/image-20250723101947159.png)





## 收发消息

![image-20250723120538000](Rabbit.assets/image-20250723120538000.png)

## work模型

![image-20250723122824124](Rabbit.assets/image-20250723122824124.png)

## 交换机的作用

![image-20250724101005176](Rabbit.assets/image-20250724101005176.png)

## 规则交换机

![image-20250724101101980](Rabbit.assets/image-20250724101101980.png)



## Topic交换机

![image-20250724103732110](Rabbit.assets/image-20250724103732110.png)

## java客户端自动创建queue的两种方式

基于注解

```java
 @RabbitListener(bindings = @QueueBinding(
            value = @Queue(name = "kumu1"),
            exchange = @Exchange(name = "suku.dircet", type = ExchangeTypes.DIRECT),
            key = {"red", "bule"}
    ))
    public void listener(String msg) {
        System.out.println("收到了一条消息" + msg + "======================");
    }
```



基于配置类

```java
package com.itheima.consumer.config;


import lombok.extern.slf4j.Slf4j;
import org.springframework.amqp.core.*;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@Slf4j
public class fanoutConfig {
    @Bean
    public FanoutExchange fanoutExchange() {
//        ExchangeBuilder.fanoutExchange("suku").build();
        return new FanoutExchange("suku");
    }

    @Bean
    public Queue queueA() {
        return new Queue("ku11");
    }

    @Bean
    public Binding bindingA(Queue queueA, FanoutExchange fanoutExchange) {
        return BindingBuilder.bind(queueA).to(fanoutExchange);
    }

}
```

## 消息格式转换

```java
    @Bean
    public Jackson2JsonMessageConverter jackson2MessageConverter() {
        return new Jackson2JsonMessageConverter();
    }
```

![image-20250724121550727](Rabbit.assets/image-20250724121550727.png)
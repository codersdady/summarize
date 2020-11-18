# bean

## 普通bean

```xml
<bean id="A" class="com.A">
```

spring直接创建A实例，并返回实例对象。

## FactoryBean

特殊bean，具有工厂生成对象能力，只能生成特定的对象。bean必须使用FactoryBean接口，此接口提供方法getObject()，生成特定bean。

> BeanFactory与FactoryBean对比：
>
> BeanFactory：工厂，用于生成bean。
>
> FactoryBean：特殊bean，用于生成另一个特定的bean。例如ProxyFactoryBean，此工厂bean用于生产代理。<bean id="" class="com.ProxyFactoryBean">并获得代理对象实例，AOP使用

# bean的作用域

- singleton：单例
- prototype：每执行一次getBean获得一个实例（struts整合spring）
- request：每个http请求创建一个新bean
- session：同一个Http session共享一个bean
- globalSession

scope配置

# bean的生命周期

createBeanInstance() ：（init-method）用于配置初始化方法，准备数据等。

populateBean()：属性赋值

initializeBean()：初始化

ConfigurableApplicationContext#close()：（destroy-methos）用于配置销毁方法，清理资源等。（1.容器必须close，销毁方法执行。2.必须是单例）
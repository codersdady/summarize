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


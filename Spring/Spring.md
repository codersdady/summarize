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

[请别再问Spring Bean的生命周期了！](https://www.jianshu.com/p/1dec08d290c1)

```java
// 忽略了无关代码
protected Object doCreateBean(final String beanName, final RootBeanDefinition mbd, final @Nullable Object[] args)
      throws BeanCreationException {

   // Instantiate the bean.
   BeanWrapper instanceWrapper = null;
   if (instanceWrapper == null) {
       // 实例化阶段！
      instanceWrapper = createBeanInstance(beanName, mbd, args);
   }

   // Initialize the bean instance.
   Object exposedObject = bean;
   try {
       // 属性赋值阶段！
      populateBean(beanName, mbd, instanceWrapper);
       // 初始化阶段！
      exposedObject = initializeBean(beanName, exposedObject, mbd);
   }
   }
```

createBeanInstance() ：实例化（init-method）用于配置初始化方法，准备数据等。

populateBean()：属性赋值

initializeBean()：初始化

ConfigurableApplicationContext#close()：（destroy-methos）用于配置销毁方法，清理资源等。（1.容器必须close，销毁方法执行。2.必须是单例）

实例化 -> 属性赋值 -> 初始化 -> 销毁

#### 第一大类：影响多个Bean的接口

实现了这些接口的Bean会切入到多个Bean的生命周期中。正因为如此，这些接口的功能非常强大，Spring内部扩展也经常使用这些接口，例如自动注入以及AOP的实现都和他们有关。

- BeanPostProcessor
- InstantiationAwareBeanPostProcessor

![spring](pic\spring.webp)

InstantiationAwareBeanPostProcessor实际上继承了BeanPostProcessor接口，严格意义上来看他们不是两兄弟，而是两父子。但是从生命周期角度我们重点关注其特有的对实例化阶段的影响，图中省略了从BeanPostProcessor继承的方法。

#### 第二大类：只调用一次的接口

1. Aware类型的接口
2. 生命周期接口

Aware

- Aware Group1
  - BeanNameAware
  - BeanClassLoaderAware
  - BeanFactoryAware
- Aware Group2
  - EnvironmentAware
  - EmbeddedValueResolverAware
  - ApplicationContextAware(ResourceLoaderAware\ApplicationEventPublisherAware\MessageSourceAware)

生命周期

- InitializingBean
- DisposableBean
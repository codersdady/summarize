# Spring Boot

Spring Boot公开三个注解：

@Configuration

@ComponentScan

@EnableAutoConfiguration

前两个spring自带，后一个为核心，可以根据类路径下的jar包和配置动态加载配置和注入bean。

@EnableAutoConfigration 注解会导入一个自动配置选择器去扫描每个jar包的META-INF/xxxx.factories 这个文件，这个文件是一个key-value形式的配置文件，里面存放了这个jar包依赖的具体依赖的自动配置类。这些自动配置类又通过@EnableConfigurationProperties 注解支持通过xxxxProperties 读取application.properties/application.yml属性文件中我们配置的值。如果我们没有配置值，就使用默认值，这就是所谓约定>配置的具体落地点

1. 自动配置，约定大于管理，即遵循“习惯优于配置”，直接使用默认配置即可。

2. 依赖管理，管理jar方便。

3. 内部集成servlet容器，jara -jar
4. 可以整合多种框架，有很好的集成。
5. 提供maven，引包方便。


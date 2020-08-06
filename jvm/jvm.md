# JVM

## JDK是什么

- JDK 是用于支持 Java 程序开发的最小环境。

- 是功能齐全的JavaSDK，拥有JRE所拥有的一切，还有编译器（javac）和工具。能够创建和编译程序。

## JRE是什么

- JRE是Java运行时环境。
- 运行已编译Java程序所需要的所有内容的集合。
- 包括JVM，Java类库，Java命令和其他的一些基础构建。
- 不能创建新程序。

## JVM

- JVM是运行Java字节码的虚拟机。
- JVM有针对不同系统的实现，使得使用相同字节码，都会给出相同的结果。

## 字节码

- JVM可以理解的代码叫字节码（.class文件），只面向虚拟机。

## 编译型语言

- 执行之前，需要编译过程，把源码编译成机器语言的文件。

- 以后运行不需要编译，执行效率高。

  ![1](pic/1.webp)

## 解释型语言

- 使用专门解释器对源程序逐行解释成特定平台的机器码并立即执行。

- 在执行时才被解释器一行行动态翻译和执行。

- 不需要事先编译，需要解释器。

- 每次将源代码解释成机器码并执行，效率低。

- Java需要编译，但编译后不能直接运行。

  ![](pic/2.webp)

## 永久代和元空间

- 字符串存在永久代中，容易出现性能问题和内存溢出。
- 类及方法的信息等比较难确定其大小，因此对于永久代的大小指定比较困难，太小容易出现永久代溢出，太大则容易导致老年代溢出。
- 永久代会为 GC 带来不必要的复杂度，并且回收效率偏低。

## 常量池

- 在 jdk1.6（含）之前**运行时常量池逻辑包含字符串常量池**存放在方法区, 此时**hotspot**虚拟机对方法区的实现为**永久代**
- 常量池在 jdk1.7（含）之后字符串常量池被从**方法区拿到了堆**中, 这里没有提到运行时常量池,也就是说字符串常量池被单独拿到堆,**运行时常量池剩下的东西还在方法区**, 也就是hotspot中的永久代 
- jdk1.8 移除了**永久代**用**元空间(Metaspace)**取而代之, 这时候**字符串常量池还在堆**, **运行时常量池还在方法区**, 只不过方法区的实现从永久代变成了元空间(Metaspace) 



# 基础

## Java的特点

+ 平台独立性 移植性  稳健性

> 将编写好的Java文件编译成.class文件, 打成jar包,就可以在不同平台上运行了

> Java是一个强类型语言 且可以使用try/catch/finally语句捕获异常

## Java是如何实现跨平台的？

Java是通过JVM（Java虚拟机）实现跨平台的JVM将字节码文件(.class)翻译成机器码才能执行

## Java和C++的区别?

+ Java 通过虚拟机从而实现跨平台特性， C++ 依赖于特定的平台
+ Java 没有指针，它的引用可以理解为安全指针，而 C++ 具有和 C 一样的指针
+ Java 支持自动垃圾回收，而 C++ 需要手动回收

## JDK/JRE/JVM的区别?

+ JVM, 是虚拟机, 是Java跨平台运行的核心
+ JRE(Java运行时环境) = JVM + Java 核心类库
+ JDK(Java开发工具包) = JRE + Java工具 + 编译器 + 调试器

## 面向对象有哪些特点?

+ 封装: 隐藏类的内部信息, 外部不允许直接访问
+ 继承: 从已有的类中派生出新的类, 提高程序的重用性和易维护性
+ 多态: 同一个行为的不同表现形式
+ 抽象: 把客观事物用代码抽象出来

## String, StringBuffer 和 StringBuilder区别

+ String 不可变，因此是线程安全的
+ StringBuilder 不是线程安全的
+ StringBuffer 是线程安全的，内部使用 synchronized 进行同步

##  什么是StringJoiner？

> StringJoiner是 Java 8 新增的一个 API，它基于 StringBuilder 实现，用于实现对字符串之间通过分隔符拼接的场景

```java
// 而通过StringJoiner来实现拼接List的各个元素，代码看起来更加简洁
List<Integer> values = Arrays.asList(1, 3, 5);
StringJoiner sj = new StringJoiner(",", "(", ")");

for (Integer value : values) {
	sj.add(value.toString());
}
```



## Java创建对象有几种方式？

Java创建对象有以下几种方式：

+ 用new语句创建对象
+ 使用反射，使用Class.newInstance()创建对象
+ 调用对象的clone()方法
+ 运用反序列化手段，调用java.io.ObjectInputStream对象的readObject()方法

## throw和throws的区别？

+ **throw**：用于抛出一个具体的异常对象
+ **throws**：用在方法签名中，用于声明该方法可能抛出的异常。子类方法抛出的异常范围更加小，或者根本不抛异常

##  如何实现对象克隆？

+ 实现`Cloneable`接口，重写 `clone()` 方法

    > 这种方式是浅拷贝，即如果类中属性有自定义引用类型，只拷贝引用，不拷贝引用指向的对象。
    >
    > 如果对象的属性的Class也实现 `Cloneable` 接口，那么在克隆对象时也会克隆属性，即深拷贝

+ 通过`org.apache.commons`中的工具类`BeanUtils`和`PropertyUtils`进行对象复制

# 集合

##  Arraylist 和 Vector 的区别

1. ArrayList在内存不够时扩容为原来的1.5倍，Vector是扩容为原来的2倍
1. Vector属于线程安全级别的，但是大多数情况下不使用Vector，因为操作Vector效率比较低




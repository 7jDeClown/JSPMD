[TOC]

# javaBean

## javaBean是什么

就是一个实体类，然后modle one模式

## javaBean规范

1. 公共的无参构造方法

   一个JavaBean对象必须拥有一个公共类型和默认的无参构造方法，从而可以通过new关键字直接对其实例化。

2. 类的声明是非final类型

   当一个类声明为final类型时，它是不可以更改的，所以JavaBean对象的声明应该是非final类型。

3. 实现可序列接口

   JavaBean应该直接或间接实现java.io.Serializable接口，以支持序列化机制。

4. 为属性声明访问器

​    JavaBean中的属性应该设置为private（私有）类型，为了防止外部直接访问，需要对外提供公共（public）的访问方法，即需要为属性提供getter/setter方法
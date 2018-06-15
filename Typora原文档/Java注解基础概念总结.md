# Java注解基础概念总结	

## 1、注解的概念

   注解（Annotation），也叫元数据（Metadata），是Java5的新特性，JDK5引入了Metadata很容易的就能够调用Annotations。注解与类、接口、枚举在同一个层次，并可以应用于包、类型、构造方法、方法、成员变量、参数、本地变量的声明中，用来对这些元素进行说明注释

```java
/*
什么是元数据?
　　元数据是指用来描述数据的数据，更通俗一点，就是描述代码间关系，或者代码与其他资源（例如数据库表）之间内在联系的数据。在一些技术框架，如struts、EJB、hibernate就不知不觉用到了元数据。对struts来说，元数据指的是struts-config.xml;对EJB来说，就是ejb-jar.xml和厂商自定义的xml文件；对hibernate来说就是hbm文件。以上阐述的几种元数据都是基于xml文件的或者其他形式的单独配置文件。这样表示有些不便之处。一、与被描述的文件分离，不利于一致性的维护；第二、所有这样文件都是ASCII文件，没有显式的类型支持。基于元数据的广泛应用，JDK5.0引入了Annotation的概念来描述元数据。在java中，元数据以标签的形式存在于java代码中，元数据标签的存在并不影响程序代码的编译和执行
--------------------------------------------------------------------------------------------------
如何创建元数据？
　　JDK5.0出来后，java语言中就有了四种类型（TYPE），即类(class)、枚举(enum)、接口(interface)和注解(@interface)，它们是处在同一级别的。java就是通过注解来表示元数据的。
一个简单的注解定义：
*/
 public @interface MyAnnotation {
    // 定义公共的final静态属性
    int age = 25;
    // 定义公共的抽象方法
    String name();
}
```

​	![img](https://images0.cnblogs.com/i/580552/201402/272148210385774.png)

```java
/*
-------------------------------------------------------------------------------------------------
反编译字节码文件得到上面的结果，由上我们可以得到，java的注解本质上是一个接口，而且是继承了接口Annotation的接口。既然是接口，那么
	a.注解可以有成员
	  注解和接口相似，它只能定义final静态属性和公共抽象方法
	b.注解的方法
	  1.方法前默认会加上public abstract且只能由这两个修饰符修饰。属性前默认加上public static final 且只能由这些修饰符修饰。由于是final，定义时必须初始化。
	  2.在声明方法时可以定义方法的默认返回值。
	  	例如: 
　　　　　　String color() default "blue"; 
　　　　　　String[] color() default {"blue","red",......}
　　　 3.方法的返回值可以有哪些类型 
　　　　　　8种基本类型，String、Class、枚举、注解及这些类型的数组。
　　　 4.java.lang.annotation.Annotation 本身是接口，而不是注解，当使用关键字@interface 定义一个注解时，该注解隐含的继承了java.lang.annotation.Annotation接口；如果我们定义一个接口，并且让该接口继承自Annotation，那么我们定义的接口依然是接口而不是注解。综上，定义注解只能依靠@interface实现。
-------------------------------------------------------------------------------------------------
JDK提供的几个基本注解
a. @SuppressWarnings    该注解的作用是阻止编译器发出某些警告信息。
　　  它可以有以下参数: 
  　　deprecation ：过时的类或方法警告。 
  　　unchecked：执行了未检查的转换时警告。 
  　　fallthrough：当Switch程序块直接通往下一种情况而没有Break时的警告。
  　　path：在类路径、源文件路径等中有不存在的路径时的警告。
  　　serial：当在可序列化的类上缺少serialVersionUID定义时的警告。
  　　finally：任何finally子句不能完成时的警告。
 　　 all：关于以上所有情况的警告。
 b.@Deprecated
  　　该注解的作用是标记某个过时的类或方法。
 c. @Override 
  　　该注解用在方法前面，用来标识该方法是重写父类的某个方法。只是给程序员一个规范让程序员看到该注解就知道这个方法是一个重写父类的方法。没有实际用处
 --------------------------------------------------------------------------------------------------
 元注解(注解的注解)
 a. @Retention  它是被定义在一个注解类的前面，用来说明该注解的生命周期。
 　　它有以下参数： 
 　　　　 RetentionPolicy.SOURCE：注解只保留在源文件，当Java文件编译成class文件的时候，注解被遗弃
 　　　　 RetentionPolicy.CLASS：注解被保留到class文件，但jvm加载class文件时候被遗弃，这是默认的生命周期
  　　　　RetentionPolicy.RUNTIME：注解不仅被保存到class文件中，jvm加载class文件之后，仍然存在；
  b. @Target 
 　　它是被定义在一个注解类的前面，用来说明该注解可以被声明在哪些元素前。(默认可以放在任何元素之前)
 　　它有以下参数： 
　　　　ElementType.TYPE：说明该注解只能被声明在一个类、接口、枚举前。 
　　　　ElementType.FIELD：说明该注解只能被声明在一个类的字段前。
　　　　ElementType.METHOD：说明该注解只能被声明在一个类的方法前。
　　　　ElementType.PARAMETER：说明该注解只能被声明在一个方法参数前。 
　　　　ElementType.CONSTRUCTOR：说明该注解只能声明在一个类的构造方法前。
　　　　ElementType.LOCAL_VARIABLE：说明该注解只能声明在一个局部变量前。
　　　　ElementType.ANNOTATION_TYPE：说明该注解只能声明在一个注解类型前。
　　　　ElementType.PACKAGE：说明该注解只能声明在一个包名前。
　 c. @Inherited 表明该注解将会被子类继承。
 　　　　需要说明的是，加上该元注解的注解，只有用在类元素上才有效果。这是在JDK总的原话:
　　　　Note that this meta-annotation type has no effect if the annotated
　　　　type is used to annotate anything other than a class. Note also
　　　　that this meta-annotation only causes annotations to be inherited
　　　　from superclasses; annotations on implemented interfaces have no
　　　　effect
　　　　但是在其他元素上的注解，只要你没有覆盖父类中的元素，是会继承过来的。这就是为什么有getDeclaredAnnotations()和getAnnotations()的原因。
　 d. @Documented
　　　　表明在生成JavaDoc文档时，该注解也会出现在javaDoc文档中。
----------------------------------------------------------------------------------------
注解的生命周期
　　一个注解可以有三个生命周期，它默认的生命周期是保留在一个CLASS文件，
 　　但它也可以由一个@Retetion的元注解指定它的生命周期。
 　　a.java源文件
  　　　　当在一个注解类前定义了一个@Retetion(RetentionPolicy.SOURCE)的注解，那么说明该注解只保留在一个源文件当中，当编译器将源文件编译成class文件时，它不会将源文件中定义的注解保留在class文件中。
　　 b. class文件中 
 　　 当在一个注解类前定义了一个@Retetion(RetentionPolicy.CLASS)的注解，那么说明该注解只保留在一个class文件当中，当加载class文件到内存时，虚拟机会将注解去掉，从而在程序中不能访问。
 　　c. 程序运行期间 
 　　 当在一个注解类前定义了一个@Retetion(RetentionPolicy.RUNTIME)的注解，那么说明该注解在程序运行期间都会存在内存当中。此时，我们可以通过反射来获得定义在某个类上的所有注解。
*/
```


---
layout: post
title: "通过JVM指令理解Java Extension"
description: ""
category: blog
tags: [Java核心技术]
---
{% include JB/setup %}
*为什么写这么一篇博客？这要从某次面试时的笔试题说起了，有感于对Java继承体系仍停留于模糊的理论阶段，于是查经阅典理解一番，遂于上月成本文之梗概，补充完整希望于人于已有所助益。*

## 理论篇：

### JVM字节码指令简要介绍

* 访问属性（原文摘自 vm spec 3.11.5章节）

    > Access fields of classes (staticfields, known as class variables) and fields of class instances (non-staticfields, known as instance variables):getfield,putfield,getstatic,putstatic.

    *注意：在访问属性时有class关键字即可，后续分析时会引用到。*
                                                                                                                                                                  
* 方法调用（原文摘自vm spec 3.11.8章节）

    > The following four instructions invoke methods:
    >
    > invokevirtualinvokes an instance method of an object, dispatching on the (virtual) type of the object. This is the normal method dispatch in the Java programming language.
    >
    > invokeinterfaceinvokes a method that is implemented by an interface, searching the methods implemented by the particular runtime object to find the appropriate method.
    >
    > invokespecialinvokes an instance method requiring special handling, whether an instance initialization method，aprivatemethod, or a superclass method.
    >
    > invokestaticinvokes a class (static) method in a named class.

    * invokevirtual 调用对象的实例方法；
    * invokeinterface 调用由接口实现的方法，在运行时对象中找到相应的实现；
    * invokespecial 调用需要特殊处理的实例方法，即实例的初始化`<init>`、private方法或超类的方法；
    * invokestatic 调用静态方法。

### 类的实例化（原文摘自vm spec 2.17.6章节）

> Whenever a new class instance is created, memory space is allocated for it with room for all the instance variables declared in the class type and all the instance variables declared in each superclass of the class type, including all the instance variables that may be hidden. If there is not sufficient space available to allocate memory for the object, then creation of the class instance completes abruptly with anOutOfMemoryError. Otherwise, all the instance variables in the new object, including those declared in superclasses, are initialized to their default values.
 
当一个实例对象被创建时，会为所有类声明的属性及其超类声明的属性分配足够的内存空间，这些属性都将被初始化。
 
> Just before a reference to the newly created object is returned as the result, the indicated constructor is processed to initialize the new object using the following procedure:
>
> Assign the arguments for the constructor to newly created parameter variables for this constructor invocation.
>
> If this constructor begins with an explicit constructor invocation of another constructor in the same class (usingthis), then evaluate the arguments and process that constructor invocation recursively using these same five steps. If that constructor invocation completes abruptly, then this procedure completes abruptly for the same reason. Otherwise, continue with step 5.
>
> If this constructor does not begin with an explicit constructor invocation of another constructor in the same class (usingthis) and is in a class other thanObject,then this constructor will begin with an explicit or implicit invocation of a superclass constructor (usingsuper). Evaluate the arguments and process that superclass constructor invocation recursively using these same five steps. If that constructor invocation completes abruptly, then this procedure completes abruptly for the same reason. Otherwise, continue with step 4.
>
> Execute the instance variable initializers for this class, assigning their values to the corresponding instance variables, in the left-to-right order in which they appear textually in the source code for the class. If execution of any of these initializers results in an exception, then no further initializers are processed and this procedure completes abruptly with that same exception. Otherwise, continue with step 5. (In some early implementations, the compiler incorrectly omitted the code to initialize a field if the field initializer expression was a constant expression whose value was equal to the default initialization value for its type. This was a bug.)
>
> Execute the rest of the body of this constructor. If that execution completes abruptly, then this procedure completes abruptly for the same reason. Otherwise, this procedure completes normally.
         
只要注意：在一个构造器开始时，总是会显式或者隐式调用父类的构造器。之后会为这些属性值设置相当的初始值。最后执行构造器中剩余的内容。

### 静态绑定与动态绑定

* 静态绑定（Static Binding）

    如果是private、static或者final方法或者构造器，那么编译时就可以准备知道应该调用哪个方法，这种调用方式称为静态绑定。对应的字节码指令为invokespecial、invokestatic。

* 动态绑定（Dynamic Binding）与方法表（Method Table）
    
    与静态绑定相对，方法调用在运行时才能决定的，就将在运行时进行动态绑定。对应的指令为invokevirtual。

    若为动态绑定，运行时虚拟机一定会调用与该对象的实际类型最合适的那个类的方法。简单的说，假设对象cat的类Cat，它是Animal类的子类。如果Cat类定义了方法bark(String)方法，那就直接调用它，否则在超类Animal中查找该方法，以此类推。

    为了方便方法调用的查找，JVM会预先为每个类创建一个方法表，包含：1）从超类继承未被覆盖的；2）从超类继承的被覆盖的；3）该类新增的。

    *注：具体的由后续的实践篇进行演示解析。*

## 实践篇：

### 验证内容

* 某子类初始化时，会分配包含超类属性的空间大小。
* 子类初始化时，总会显式或者隐式调用超类的构造器。
* 若子类与父类包含相同名称的属性时，如何取属性以进行处理?
* private、构造方法会通过invokespecial指令调用；static方法会通过invokestatic指令调用；一般对象方法会通过invokevirtual指令进行调用。

### 示例代码

类Animal：

    // TODO 添加Animail源码

类Cat（继承Animal）：

    // TODO 添加Cat源码
 
### 示例说明

参考之前简要介绍的理论基础，再运行Cat.main()方法验证猜想。下边对照示例代码的字节码指令简要的说明一下。

* 类的初始化：

        0:  aload_0  
        1:  invokespecial   #11; //Method blog/arvin_xiaoh/y11m10/Animal."<init>":()V  
        4:  aload_0  
        5:  ldc #13; //String ?  
        7:  putfield    #15; //Field tag:Ljava/lang/String;  
        10: aload_0  
        11: ldc #17; //String Tom  
        13: putfield    #19; //Field name:Ljava/lang/String;  
        16: aload_0  
        17: ldc #21; //String TomCat  
        19: putfield    #19; //Field name:Ljava/lang/String;  
        22: return  

    以上字节码指令为Cat类构造器指令:

    注意1 - invokespecial 调用父类的构造，验证了子类总是会在显式或者隐式调用父类的构造。

    而4～13为编译器提供的属性的初始化，验证了理论篇中提及的，在调用父类构造后会为属性进行初始化操作。
 
    而16～19为重新设置name值为Tomcat。
 
    由此再结合Animal的构造器指令，其实也可以猜测到Animal的构造里也会对其属性进行初始化操作，故也在一定程度上验证了该对象会包含父类属性的观点。（同时由于在Animal中特意没有给age提供初始值，可以发现在初始化时会忽略该属性）

* Cat.eat()，查看super的方法调用

        0:  getstatic   #28; //Field java/lang/System.out:Ljava/io/PrintStream;  
        3:  ldc #43; //String ????^_^...  
        5:  invokevirtual   #36; //Method java/io/PrintStream.println:(Ljava/lang/String;)V  
        8:  aload_0  
        9:  invokespecial   #45; //Method blog/arvin_xiaoh/y11m10/Animal.eat:()V  
        12: return

    重点看第9行，super.eat()会被编译成invokespecial，并与Animal.eat()方法直接相关。也就是说super.eat()其实就是Animal.eat()，在编译期就已经确定，是静态绑定的方式。
    
    理解了这点，对于sleep2()的理解也就豁然开朗了。

* cat.sleep()，动态绑定的示例。

        0:  aload_0  
        1:  invokevirtual   #41; //Method sleepBody:()V  
        4:  return  

    以上是Animal.sleep()方法体的字节码指令，很简单，着重注意invokevirtual，由于当前对象cat在其类Cat中包含了sleepBody()，故其会与Cat类中的方法关联，而sleepBody()中的sleepContent()总是invokespecial，故最后显示的是Cat.sleepContent()的内容。

* 综合上述内容，cat.sleep2()的理解就按下不表。


再上传eclipse中debug中查看cat对象的属性内容，以及使用jhat＆jmap查看的堆中对象内容截图如下，都论证了对象会包含超类的属性：

---
layout: post
title: "Spring AOP Aspect"
date: 2016-03-25 10:52:14 +0800
comments: true
categories: 
---
##AOP的实现中，会有代码织入（weaver）的概念，根据织入的时间节点不同，可以分为四种：
Compile-time weaving：当你完成了源代码的编写，利用Aspectj的ajc命令，以source code为输入，输出织入好的class文件。
Post-compile weaving：是为了weaver已经存在的class文件和Jar文件，这发生在使用或部署程序之前
Load-time weaving (LTW)：这个过程，直到使用classloader加载class文件时才发生，所以在运行环境中要存在“weaving class loader”或者使用一个织入代理
run-time weaving：这个过程发生时，class已经在JVM中有定义，织入时无需再加载class.像Spring AOP这种纯AOP框架使用的是run-time weaving.

##Aspectj支持前三种织入方式
使用AspectJ LTW有两个主要步骤，第一，通过JVM的-javaagent参数设置LTW的织入器类包，以代理JVM默认的类加载器；第二，LTW织入器需要一个 aop.xml文件，在该文件中指定切面类和需要进行切面织入的目标类


##Spring AOP不同于大多数其他AOP框架。
Spring AOP的目的并不是为了提供最完整的AOP实现（虽然Spring AOP具有相当的能力）；而是为了要帮助解决企业应用中的常见问题，提供一个AOP实现与Spring IOC之间的紧密集成。由于Spring AOP是容易实现的，如果你计划在Spring Beans之上将横切关注点模块化，

在决定使用哪种框架实现你的项目之前，有几个要点可以帮助你做出合适的选择（同样适用于其他框架）。

Spring AOP致力于提供一种能够与Spring IoC紧密集成的面向方面框架的实现，以便于解决在开发企业级项目时面临的常见问题。明确你在应用横切关注点(cross-cutting concern)时（例如事物管理、日志或性能评估），需要处理的是Spring beans还是POJO。如果正在开发新的应用，则选择Spring AOP就没有什么阻力。但是如果你正在维护一个现有的应用（该应用并没有使用Spring框架），AspectJ就将是一个自然的选择了。为了详细说明这一点，假如你正在使用Spring AOP，当你想将日志功能作为一个通知(advice)加入到你的应用中，用于追踪程序流程，那么该通知(Advice)就只能应用在Spring beans的连接点(Joinpoint)之上。

 spring aop是aop实现方案的一种，它支持在运行期基于动态代理的方式将aspect织入目标代码中来实现aop。但是spring aop的切入点支持有限，而且对于static方法和final方法都无法支持aop（因为此类方法无法生成代理类）
 
 Spring AOP提供了较通用的编写Aspect,pointcuts的方式，使用@Aspect、@Pointcut类似的注解或者XML配置就可以完成面向切面编程，这对于你是使用Spring AOP还是使用Aspectj都是一致生效的。
 
 
##下面是AOP的一些基本概念，转载自[http://jinnianshilongnian.iteye.com/blog/1474325](http://jinnianshilongnian.iteye.com/blog/1474325)

###AOP入门
####关注点：
可以是所关注的任何东西，比如支付、日志及、事物
####关注点分离
将问题细化为单独的部分，即可以理解为不可再分割的组件，如日志组件和支付组件
####横切关注点：会在多个模块中出现，使用现有的编程方法，横切关注点会横跨多个模块，结果是使系统难以设计、理解、实现和演进，如日志组件横切与支付组件
####织入
横切关注点分离后，需要通过某种技术将横切关注点融合到系统中从而完成需要的功能，因此需要织入
###Aop是什么
AOP是一种编程范式，提供了另一个角度来考虑程序结构以完善面向对象编程（OOP）
AOP为开发者提供了一种描述横切关注点的机制，并能够自动将横切关注点织入到面向对象的软件系统中，从而实现横切关注点的模块化
AOP能够将那些与业务无关，却为业务模块共同调用的逻辑或责任，例如事物管理、日志管理、权限管理等，封装起来，以减少系统的重复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性
###AOP能干什么，及好处
降低模块的耦合度
使系统容易扩展
设计决定的迟绑定：可以把这方面的需求作为独立的方面来实现
更好的代码复用


###AOP基本概念
 连接点（Joinpoint）：
 
    表示需要在程序中插入横切关注点的扩展点，连接点可能是类初始化、方法执行、方法调用、字段调用或处理异常等等，Spring只支持方法执行连接点，在AOP中表示为“在哪里做”；
    
切入点（Pointcut）：

    选择一组相关连接点的模式，即可以认为连接点的集合，Spring支持perl5正则表达式和AspectJ切入点模式，Spring默认使用AspectJ语法，在AOP中表示为“在哪里做的集合”；
    
增强（Advice）：或称为增强

    在连接点上执行的行为，增强提供了在AOP中需要在切入点所选择的连接点处进行扩展现有行为的手段；包括前置增强（before advice）、后置增强 (after advice)、环绕增强 （around advice），在Spring中通过代理模式实现AOP，并通过拦截器模式以环绕连接点的拦截器链织入增强 ；在AOP中表示为“做什么”；
    
方面/切面（Aspect）：

      横切关注点的模块化，比如上边提到的日志组件。可以认为是增强、引入和切入点的组合；在Spring中可以使用Schema和@AspectJ方式进行组织实现；在AOP中表示为“在哪里做和做什么集合”；
      
目标对象（Target Object）：

    需要被织入横切关注点的对象，即该对象是切入点选择的对象，需要被增强的对象，从而也可称为“被增强对象”；由于Spring AOP 通过代理模式实现，从而这个对象永远是被代理对象，在AOP中表示为“对谁做”；
AOP代理（AOP Proxy）：
    AOP框架使用代理模式创建的对象，从而实现在连接点处插入增强（即应用切面），就是通过代理来对目标对象应用切面。在Spring中，AOP代理可以用JDK动态代理或CGLIB代理实现，而通过拦截器模型应用切面。
    
织入（Weaving）：
    织入是一个过程，是将切面应用到目标对象从而创建出AOP代理对象的过程，织入可以在编译期、类装载期、运行期进行。
    
引入（inter-type declaration）：

    也称为内部类型声明，为已有的类添加额外新的字段或方法，Spring允许引入新的接口（必须对应一个实现）到所有被代理对象（目标对象）, 在AOP中表示为“做什么（新增什么）”；
    
   
AOP的Advice类型

前置增强（Before advice）：
    在某连接点之前执行的增强，但这个增强不能阻止连接点前的执行（除非它抛出一个异常）。
    
后置返回增强（After returning advice）：
    在某连接点正常完成后执行的增强：例如，一个方法没有抛出任何异常，正常返回。
    
后置异常增强（After throwing advice）：
    在方法抛出异常退出时执行的增强。
    
后置最终增强（After (finally) advice）：
    当某连接点退出的时候执行的增强（不论是正常返回还是异常退出）。
    
环绕增强（Around Advice）：
    包围一个连接点的增强，如方法调用。这是最强大的一种增强类型。 环绕增强可以在方法调用前后完成自定义的行为。它也会选择是否继续执行连接点或直接返回它们自己的返回值或抛出异常来结束执行。




































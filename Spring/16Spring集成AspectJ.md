Spring AOP 是一个简化版的 AOP 实现，并没有提供完整版的 AOP 功能。通常情况下，Spring AOP 是能够满足我们日常开发过程中的大多数场景的，但在某些情况下，我们可能需要使用 Spring AOP 范围外的某些 AOP 功能。

例如 Spring AOP 仅支持执行公共（public）非静态方法的调用作为连接点，如果我们需要向受保护的（protected）或私有的（private）的方法进行增强，此时就需要使用功能更加全面的 AOP 框架来实现，其中使用最多的就是 AspectJ。

AspectJ 是一个基于 Java 语言的全功能的 AOP 框架，它并不是 Spring 组成部分，是一款独立的 AOP 框架。

但由于 AspectJ 支持通过 Spring 配置 AspectJ 切面，因此它是 Spring AOP 的完美补充，通常情况下，我们都是将 AspectJ 和 Spirng 框架一起使用，简化 AOP 操作。

使用 AspectJ 需要在 Spring 项目中导入 Spring AOP 和 AspectJ 相关 Jar 包。

- spring-aop-xxx.jar
- spring-aspects-xxx.jar
- aspectjweaver-xxxx.jar

在以上 3 个 Jar 包中，spring-aop-xxx.jar 和 spring-aspects-xxx.jar 为 Spring 框架提供的 Jar 包，而 aspectjweaver-xxxx.jar 则是 AspectJ 提供的。

## Spring使用AspectJ进行AOP开发（基于XML）

Spring 项目中通过 XML 配置，对切面（Aspect 或 Advisor）、切点（PointCut）以及通知（Advice）进行定义和管理，以实现基于 AspectJ 的 AOP 开发。

Spring 提供了基于 XML 的 AOP 支持，并提供了一个名为“aop”的命名空间，该命名空间提供了一个` <aop:config> `元素。

- 在 Spring 配置中，所有的切面信息（切面、切点、通知）都必须定义在 `<aop:config>` 元素中；
- 在 Spring 配置中，可以使用多个` <aop:config>`。
- 每一个 `<aop:config>` 元素内可以包含 3 个子元素： pointcut、advisor 和 aspect ，这些子元素必须按照这个顺序进行声明。

### 引入 aop 命名空间

```XML
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/aop
    http://www.springframework.org/schema/aop/spring-aop-3.0.xsd ">
    ...
</beans>
```

### 定义切面

在 Spring 配置文件中，使用` <aop:aspect> `元素定义切面。该元素可以将定义好的 Bean 转换为切面 Bean，所以使用` <aop:aspect> `之前需要先定义一个普通的 Spring Bean。

```XML
<aop:config>
    <aop:aspect id="myAspect" ref="aBean">
        ...
    </aop:aspect>
</aop:config>
```

其中，id 用来定义该切面的唯一标识名称，ref 用于引用普通的 Spring Bean。

### 定义切入点

`<aop:pointcut> `用来定义一个切入点，用来表示对哪个类中的那个方法进行增强。它既可以在 <aop:pointcut> 元素中使用，也可以在` <aop:aspect> `元素下使用。

- 当` <aop:pointcut>`元素作为 `<aop:config> `元素的子元素定义时，表示该切入点是全局切入点，它可被多个切面所共享；
- 当` <aop:pointcut> `元素作为` <aop:aspect>` 元素的子元素时，表示该切入点只对当前切面有效。
Spring AOP 是 Spring 框架的核心模块之一，它使用纯 Java 实现，因此不需要专门的编译过程和类加载器，可以在程序运行期通过代理方式向目标类织入增强代码。

## Spring AOP 的代理机制

Spring 在运行期会为目标对象生成一个动态代理对象，并在代理对象中实现对目标对象的增强。

Spring AOP 的底层是通过以下 2 种动态代理机制，为目标对象（Target Bean）执行横向织入的。

| 代理技术       | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| JDK 动态代理   | Spring AOP 默认的动态代理方式，若目标对象实现了若干接口，Spring 使用 JDK 的 java.lang.reflect.Proxy 类进行代理。 |
| CGLIB 动态代理 | 若目标对象没有实现任何接口，Spring 则使用 CGLIB 库生成目标对象的子类，以实现对目标对象的代理。 |

> 注意：由于被标记为 final 的方法是无法进行覆盖的，因此这类方法不管是通过 JDK 动态代理机制还是 CGLIB 动态代理机制都是无法完成代理的。

## Spring AOP 连接点

Spring AOP 并没有像其他 AOP 框架（例如 AspectJ）一样提供了完成的 AOP 功能，它是 Spring 提供的一种简化版的 AOP 组件。其中最明显的简化就是，Spring AOP 只支持一种连接点类型：方法调用。您可能会认为这是一个严重的限制，但实际上 Spring AOP 这样设计的原因是为了让 Spring 更易于访问。

方法调用连接点是迄今为止最有用的连接点，通过它可以实现日常编程中绝大多数与 AOP 相关的有用的功能。如果需要使用其他类型的连接点（例如成员变量连接点），我们可以将 Spring AOP 与其他的 AOP 实现一起使用，最常见的组合就是 Spring AOP + ApectJ。 

## Spring AOP 通知类型

AOP 联盟为通知（Advice）定义了一个 org.aopalliance.aop.Interface.Advice 接口。

Spring AOP 按照通知（Advice）织入到目标类方法的连接点位置，为 Advice 接口提供了 6 个子接口，如下表。

| 通知类型     | 接口                                            | 描述                                             |
| ------------ | ----------------------------------------------- | ------------------------------------------------ |
| 前置通知     | org.springframework.aop.MethodBeforeAdvice      | 在目标方法执行前实施增强。                       |
| 后置通知     | org.springframework.aop.AfterReturningAdvice    | 在目标方法执行后实施增强。                       |
| 后置返回通知 | org.springframework.aop.AfterReturningAdvice    | 在目标方法执行完成，并返回一个返回值后实施增强。 |
| 环绕通知     | org.aopalliance.intercept.MethodInterceptor     | 在目标方法执行前后实施增强。                     |
| 异常通知     | org.springframework.aop.ThrowsAdvice            | 在方法抛出异常后实施增强。                       |
| 引入通知     | org.springframework.aop.IntroductionInterceptor | 在目标类中添加一些新的方法和属性。               |

## Spring AOP 切面类型

Spring 使用 org.springframework.aop.Advisor 接口表示切面的概念，实现对通知（Adivce）和连接点（Joinpoint）的管理。

在 Spring AOP 中，切面可以分为三类：一般切面、切点切面和引介切面。

| 切面类型 | 接口                                        | 描述                                                         |
| -------- | ------------------------------------------- | ------------------------------------------------------------ |
| 一般切面 | org.springframework.aop.Advisor             | Spring AOP 默认的切面类型。  由于 Advisor 接口仅包含一个 Advice（通知）类型的属性，而没有定义 PointCut（切入点），因此它表示一个不带切点的简单切面。  这样的切面会对目标对象（Target）中的所有方法进行拦截并织入增强代码。由于这个切面太过宽泛，因此我们一般不会直接使用。 |
| 切点切面 | org.springframework.aop.PointcutAdvisor     | Advisor 的子接口，用来表示带切点的切面，该接口在 Advisor 的基础上还维护了一个 PointCut（切点）类型的属性。  使用它，我们可以通过包名、类名、方法名等信息更加灵活的定义切面中的切入点，提供更具有适用性的切面。 |
| 引介切面 | org.springframework.aop.IntroductionAdvisor | Advisor 的子接口，用来代表引介切面，引介切面是对应引介增强的特殊的切面，它应用于类层面上，所以引介切面适用 ClassFilter 进行定义。 |

## 一般切面的 AOP 开发

当我们在使用 Spring AOP 开发时，若没有对切面进行具体定义，Spring AOP 会通过 Advisor 为我们定义一个一般切面（不带切点的切面），然后对目标对象（Target）中的所有方法连接点进行拦截，并织入增强代码。

```java
package com.wqcctgu.www.dao;
public interface UserDao {
    public void add();
    public void delete();
    public void modify();
    public void get();
}
```

```java
package com.wqcctgu.www.dao.impl;

import com.wqcctgu.www.dao.UserDao;
public class UserDaoImpl implements UserDao {
    @Override
    public void add() {
        System.out.println("正在执行 UserDao 的 add() 方法……");
    }
    @Override
    public void delete() {
        System.out.println("正在执行 UserDao 的 delete() 方法……");
    }
    @Override
    public void modify() {
        System.out.println("正在执行 UserDao 的 modify() 方法……");
    }
    @Override
    public void get() {
        System.out.println("正在执行 UserDao 的 get() 方法……");
    }
}
```

```java
package com.wqcctgu.www.advice;
import org.springframework.aop.MethodBeforeAdvice;
import java.lang.reflect.Method;
public class UserDaoBeforeAdvice implements MethodBeforeAdvice {
    @Override
    public void before(Method method, Object[] args, Object target) throws Throwable {
        System.out.println("正在执行前置增强操作…………");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
    <!--******Advisor : 代表一般切面，Advice 本身就是一个切面，对目标类所有方法进行拦截(* 不带有切点的切面.针对所有方法进行拦截)*******-->
    <!-- 定义目标（target）对象 -->
    <bean id="userDaoImpl" class="com.wqcctgu.www.dao.impl.UserDaoImpl"></bean>
    <!-- 定义增强 -->
    <bean id="beforeAdvice" class="com.wqcctgu.www.advice.UserDaoBeforeAdvice"></bean>
    <!--通过配置生成代理 UserDao 的代理对象 -->
    <bean id="userDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
        <!-- 设置目标对象 -->
        <property name="target" ref="userDaoImpl"/>
        <!-- 设置实现的接口 ,value 中写接口的全路径 -->
        <property name="proxyInterfaces" value="com.wqcctgu.www.dao.UserDao"/>
        <!-- 需要使用value:增强 Bean 的名称 -->
        <property name="interceptorNames" value="beforeAdvice"/>
    </bean>
</beans>
```

```java
package com.wqcctgu.www;

import com.wqcctgu.www.dao.UserDao;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        //获取代理对象
        UserDao userDao = context.getBean("userDaoProxy", UserDao.class);
        //调用 UserDao 中的各个方法
        userDao.add();
        userDao.delete();
        userDao.get();
        userDao.modify();
    }
}
```


Spring 能够基于 org.springframework.aop.framework.ProxyFactoryBean 类，根据目标对象的类型（是否实现了接口）自动选择使用 JDK 动态代理或 CGLIB 动态代理机制，为目标对象（Target Bean）生成对应的代理对象（Proxy Bean）。

ProxyFactoryBean 的常用属性如下表所示。

| 属性             | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| target           | 需要代理的目标对象（Bean）                                   |
| proxyInterfaces  | 代理需要实现的接口，如果需要实现多个接口，可以通过 <list> 元素进行赋值。 |
| proxyTargetClass | 针对类的代理，该属性默认取值为 false（可省略）, 表示使用 JDK 动态代理；取值为 true，表示使用 CGlib 动态代理 |
| interceptorNames | 拦截器的名字，该属性的取值既可以是拦截器、也可以是 Advice（通知）类型的 Bean，还可以是切面（Advisor）的 Bean。 |
| singleton        | 返回的代理对象是否为单例模式，默认值为 true。                |
| optimize         | 是否对创建的代理进行优化（只适用于CGLIB）。                  |

>正在执行前置增强操作…………
>正在执行 UserDao 的 add() 方法……
>正在执行前置增强操作…………
>正在执行 UserDao 的 delete() 方法……
>正在执行前置增强操作…………
>正在执行 UserDao 的 get() 方法……
>正在执行前置增强操作…………
>正在执行 UserDao 的 modify() 方法……

## 基于 PointcutAdvisor 的 AOP 开发

PointCutAdvisor 是 Adivsor 接口的子接口，用来表示带切点的切面。使用它，我们可以通过包名、类名、方法名等信息更加灵活的定义切面中的切入点，提供更具有适用性的切面。

Spring 提供了多个 PointCutAdvisor 的实现，其中常用实现类如如下。

- NameMatchMethodPointcutAdvisor：指定 Advice 所要应用到的目标方法名称，例如 hello* 代表所有以 hello 开头的所有方法。
- RegExpMethodPointcutAdvisor：使用正则表达式来定义切点（PointCut），RegExpMethodPointcutAdvisor 包含一个 pattern 属性，该属性使用正则表达式描述需要拦截的方法。

```java
package com.wqcctgu.www.dao;
public class OrderDao {
    public void add() {
        System.out.println("正在执行 UserDao 的 add() 方法……");
    }
    public void adds() {
        System.out.println("正在执行 UserDao 的 adds() 方法……");
    }
    public void delete() {
        System.out.println("正在执行 UserDao 的 delete() 方法……");
    }
    public void modify() {
        System.out.println("正在执行 UserDao 的 modify() 方法……");
    }
    public void get() {
        System.out.println("正在执行 UserDao 的 get() 方法……");
    }
}
```

```java
package com.wqcctgu.www.advice;

import org.aopalliance.intercept.MethodInterceptor;
import org.aopalliance.intercept.MethodInvocation;
public class OrderDaoAroundAdvice implements MethodInterceptor {
    @Override
    public Object invoke(MethodInvocation methodInvocation) throws Throwable {
        System.out.println("环绕增强前********");
        //执行被代理对象中的逻辑
        Object result = methodInvocation.proceed();
        System.out.println("环绕增强后********");
        return result;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--带切点的切面-->
    <!-- 定义目标（target）对象 -->
    <bean id="orderDao" class="com.wqcctgu.www.dao.OrderDao"></bean>
    <!-- 定义增强 -->
    <bean id="aroundAdvice" class="com.wqcctgu.www.advice.OrderDaoAroundAdvice"></bean>
    <!--定义切面-->
    <bean id="myPointCutAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <!--定义表达式，规定哪些方法进行拦截 .* 表示所有方法-->
        <!--<property name="pattern" value=".*"></property>-->
        <property name="patterns" value="com.wqcctgu.www.dao.OrderDao.add.*,com.wqcctgu.www.dao.OrderDao.delete.*"></property>
        <property name="advice" ref="aroundAdvice"></property>
    </bean>
    <!--Spring 通过配置生成代理-->
    <bean id="orderDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
        <!-- 配置目标 -->
        <property name="target" ref="orderDao"></property>
        <!-- 针对类的代理,该属性默认取值为 false（可省略）, 表示使用 JDK 动态代理；取值为 true,表示使用 CGlib 动态代理-->
        <property name="proxyTargetClass" value="true"></property>
        <!-- 在目标上应用增强 -->
        <property name="interceptorNames" value="myPointCutAdvisor"></property>
    </bean>
</beans>
```

```java
package com.wqcctgu.www;
import com.wqcctgu.www.dao.OrderDao;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        //获取代理对象
        OrderDao orderDao = context.getBean("orderDaoProxy", OrderDao.class);
        //调用 OrderDao 中的各个方法
        orderDao.add();
        orderDao.adds();
        orderDao.delete();
        orderDao.get();
        orderDao.modify();
    }
}
```

>环绕增强前********
>正在执行 UserDao 的 add() 方法……
>环绕增强后********
>环绕增强前********
>正在执行 UserDao 的 adds() 方法……
>环绕增强后********
>环绕增强前********
>正在执行 UserDao 的 delete() 方法……
>环绕增强后********
>正在执行 UserDao 的 get() 方法……
>正在执行 UserDao 的 modify() 方法……

## 自动代理

在前面的案例中，所有目标对象（Target Bean）的代理对象（Proxy Bean）都是在 XML 配置中通过 ProxyFactoryBean 创建的。但在实际开发中，一个项目中往往包含非常多的 Bean， 如果每个 Bean 都通过 ProxyFactoryBean 创建，那么开发和维护成本会十分巨大。为了解决这个问题，Spring 为我们提供了自动代理机制。

Spring 提供的自动代理方案，都是基于后处理 Bean 实现的，即在 Bean 创建的过程中完成增强，并将目标对象替换为自动生成的代理对象。通过 Spring 的自动代理，我们在程序中直接拿到的 Bean 就已经是 Spring 自动生成的代理对象了。

Spring 为我们提供了 3 种自动代理方案：

- BeanNameAutoProxyCreator：根据 Bean 名称创建代理对象。
- DefaultAdvisorAutoProxyCreator：根据 Advisor 本身包含信息创建代理对象。
- AnnotationAwareAspectJAutoProxyCreator：基于 Bean 中的 AspectJ 注解进行自动代理对象。

在以上项目基础上，如下示范：

### 根据 Bean 名称创建代理对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 定义目标（target）对象 -->
    <bean id="userDaoImpl" class="com.wqcctgu.www.dao.impl.UserDaoImpl"></bean>
    <bean id="orderDao" class="com.wqcctgu.www.dao.OrderDao"></bean>
    <!-- 定义增强 -->
    <bean id="beforeAdvice" class="com.wqcctgu.www.advice.UserDaoBeforeAdvice"></bean>
    <bean id="aroundAdvice" class="com.wqcctgu.www.advice.OrderDaoAroundAdvice"></bean>
    <!--Spring 自动代理：根据 Bean 名称创建代理独享-->
    <bean class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">
        <property name="beanNames" value="*Dao*"></property>
        <property name="interceptorNames" value="beforeAdvice,aroundAdvice"></property>
    </bean>
</beans>
```

```java
package com.wqcctgu.www;
import com.wqcctgu.www.dao.OrderDao;
import com.wqcctgu.www.dao.UserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans2.xml");
        //获取代理对象
        UserDao userDao = context.getBean("userDaoImpl", UserDao.class);
        //获取代理对象
        OrderDao orderDao = context.getBean("orderDao", OrderDao.class);
        //调用 UserDao 中的各个方法
        userDao.add();
        userDao.delete();
        userDao.modify();
        userDao.get();
        //调用 OrderDao 中的各个方法
        orderDao.add();
        orderDao.adds();
        orderDao.delete();
        orderDao.get();
        orderDao.modify();
    }
}
```

### 根据切面中信息创建代理对象

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
    http://www.springframework.org/schema/context
            http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 定义目标（target）对象 -->
    <bean id="userDaoImpl" class="com.wqcctgu.www.dao.impl.UserDaoImpl"></bean>
    <bean id="orderDao" class="com.wqcctgu.www.dao.OrderDao"></bean>
    <!-- 定义增强 -->
    <bean id="beforeAdvice" class="com.wqcctgu.www.advice.UserDaoBeforeAdvice"></bean>
    <bean id="aroundAdvice" class="com.wqcctgu.www.advice.OrderDaoAroundAdvice"></bean>
    <!--定义切面-->
    <bean id="myPointCutAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <!--定义表达式，规定哪些方法进行拦截 .* 表示所有方法-->
        <!--<property name="pattern" value=".*"></property>-->
        <property name="patterns"            value="com.wqcctgu.www.dao.OrderDao.add.*,com.wqcctgu.www.dao.OrderDao.delete.*"></property>
        <property name="advice" ref="aroundAdvice"></property>
    </bean>
    <!--Spring 自动代理：根据切面 myPointCutAdvisor 中信息创建代理对象-->
    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"></bean>
</beans>
```

MainApp同上

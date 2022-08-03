# Spring后置处理器

Spring Bean生命周期中提到的 BeanPostProcessor 接口。BeanPostProcessor 接口也被称为后置处理器，通过该接口可以自定义调用初始化前后执行的操作方法。

接口源码：

```java
public interface BeanPostProcessor {
    Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException;
    Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException;
}
```

该接口中包含了两个方法：

- postProcessBeforeInitialization() 方法：在 Bean 实例化、属性注入后，初始化前调用。
- postProcessAfterInitialization() 方法：在 Bean 实例化、属性注入、初始化都完成后调用。

当需要添加多个后置处理器实现类时，默认情况下 Spring 容器会根据后置处理器的定义顺序来依次调用。也可以通过实现 Ordered 接口的 getOrder 方法指定后置处理器的执行顺序。该方法返回值为整数，默认值为 0，取值越大优先级越低。

需要注意的是，postProcessBeforeInitialization 和 postProcessAfterInitialization 方法返回值不能为 null，否则会报空指针异常或者通过 getBean() 方法获取不到 Bean 实例对象。

```java
//HelloWorld.java
package com.wqcctgu.www;
public class HelloWorld {
    private String message;
    public void setMessage(String message) {
        this.message = message;
    }
    public void getMessage() {
        System.out.println("Message : " + message);
    }
    public void init() {
        System.out.println("Bean 正在初始化");
    }
    public void destroy() {
        System.out.println("Bean 将要被销毁");
    }
}
```

```java
//MainApp.java
package com.wqcctgu.www;
import org.springframework.context.support.AbstractApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    public static void main(String[] args) {
        AbstractApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
        obj.getMessage();
        context.registerShutdownHook();
    }
}
```

```java
//InitHelloWorld.java
package com.wqcctgu.www;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.core.Ordered;
public class InitHelloWorld implements BeanPostProcessor, Ordered {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("A Before : " + beanName);
        return bean;
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("A After : " + beanName);
        return bean;
    }
    @Override
    public int getOrder() {
        return 5;
    }
}
```

```java
//InitHelloWorld2.java
package com.wqcctgu.www;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.core.Ordered;
public class InitHelloWorld2 implements BeanPostProcessor, Ordered {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("B Before : " + beanName);
        return bean;
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("B After : " + beanName);
        return bean;
    }
    @Override
    public int getOrder() {
        return 0;
    }
}
```

```xml
<!--Beans.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="helloWorld" class="com.wqcctgu.www.HelloWorld"
          init-method="init" destroy-method="destroy">
        <property name="message" value="Hello World！" />
    </bean>
    <!-- 注册处理器 -->
    <bean class="com.wqcctgu.www.InitHelloWorld" />
    <bean class="com.wqcctgu.www.InitHelloWorld2" />
</beans>
```

>由运行结果可以看出，postProcessBeforeInitialization 方法是在 Bean 实例化和属性注入后，自定义初始化方法前执行的。而 postProcessAfterInitialization 方法是在自定义初始化方法后执行的。由于 getOrder 方法返回值越大，优先级越低，因此 InitHelloWorld2 先执行
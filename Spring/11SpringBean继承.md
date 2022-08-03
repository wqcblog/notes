在 Spring 中，Bean 和 Bean 之间也存在继承关系。我们将被继承的 Bean 称为父 Bean，将继承父 Bean 配置信息的 Bean 称为子 Bean。

Spring Bean 的定义中可以包含很多配置信息，例如构造方法参数、属性值。子 Bean 既可以继承父 Bean 的配置数据，也可以根据需要重写或添加属于自己的配置信息。

在 Spring XML 配置中，我们通过子 Bean 的 parent 属性来指定需要继承的父 Bean，配置格式如下。

```xml
<!--父Bean-->
<bean id="parentBean" class="xxx.xxxx.xxx.ParentBean" >
    <property name="xxx" value="xxx"></property>
    <property name="xxx" value="xxx"></property>
</bean> 
<!--子Bean--> 
<bean id="childBean" class="xxx.xxx.xxx.ChildBean" parent="parentBean"></bean>
```

## 实例


```java
//Animal.java
package com.wqcctgu.www;
public class Animal {
    private String name;
    private Integer age;
    public void setName(String name) {
        System.out.println("Animal setName：" + name);
        this.name = name;
    }
    public void setAge(Integer age) {
        System.out.println("Animal setAge：" + age);
        this.age = age;
    }
    @Override
    public String toString() {
        return "Animal{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
//Dog.java
package com.wqcctgu.www;

public class Dog {
    private String name;
    private Integer age;
    private String call;
    public void setCall(String call) {
        System.out.println("Dog setCall：" + call);
        this.call = call;
    }
    public void setName(String name) {
        System.out.println("Dog setName：" + name);
        this.name = name;
    }
    public void setAge(Integer age) {
        System.out.println("Dog setAge：" + age);
        this.age = age;
    }
    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", call='" + call + '\'' +
                '}';
    }
}
```

```java
//MainApp.java
package com.wqcctgu.www;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    public static void main(String[] args) {
        //获取 ClassPathXmlApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        Dog dog = context.getBean("dog", Dog.class);
        System.out.println(dog);
    }
}
```

```xml
<!--Beans.xml-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="animal" class="com.wqcctgu.www.Animal">
        <property name="name" value="动物"></property>
        <property name="age" value="10"></property>
    </bean>
    <bean id="dog" class="com.wqcctgu.www.Dog" parent="animal">
        <property name="name" value="小狗"></property>
        <property name="call" value="汪汪汪……"></property>
    </bean>
</beans>
```

```
Animal setName：动物
Animal setAge：10
Dog setName：小狗
Dog setAge：10
Dog setCall：汪汪汪……
Dog{name='小狗', age=10, call='汪汪汪……'}
```

## Bean定义模板

如果一个父 Bean 的 abstract 属性值为 true，则表明这个 Bean 是抽象的。

抽象的父 Bean 只能作为模板被子 Bean 继承，它不能实例化，也不能被其他 Bean 引用，更不能在代码中根据 id 调用 getBean() 方法获取，否则就会返回错误。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="animal" abstract="true">
        <property name="name" value="动物"></property>
        <property name="age" value="10"></property>
    </bean>
    <bean id="dog" class="com.wqcctgu.www.Dog" parent="animal">
        <property name="name" value="小狗"></property>
        <property name="call" value="汪汪汪……"></property>
    </bean>
</beans>
```

```
Dog setName：小狗
Dog setAge：10
Dog setCall：汪汪汪……
Dog{name='小狗', age=10, call='汪汪汪……'}
```

通过控制台输出可以看出，在 Spring 启动时，并没有实例化 animal。
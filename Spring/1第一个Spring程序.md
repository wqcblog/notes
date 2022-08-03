## 创建Java类

```java
// HelloWorld,java
package com.wqcctgu.www;

public class HelloWorld {
	private String message;
	
	public void setMessage(String message) {
		this.message=message;
	}
	public void getMessgae() {
		System.out.println("messgae:"+message);
	}
    
}
```

```java
//MainApp.java
package com.wqcctgu.www;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainAPP {
    	public static void main(String[] args) {
    		ApplicationContext context=new ClassPathXmlApplicationContext("Beans.xml");
    		HelloWorld obj=context.getBean("helloWorld",HelloWorld.class);
     //getBean函数传入的参数值分别对应Beans文件中相应的id和class文件名
    	    obj.getMessgae();
    	}
}
//这是属性注入中提到的setter方法
```

- 创建 ApplicationContext 对象时使用了 ClassPathXmlApplicationContext 类，这个类用于加载 Spring 配置文件、创建和初始化所有对象（Bean）。
- ApplicationContext.getBean() 方法用来获取 Bean，该方法返回值类型为 Object，通过强制类型转换为 HelloWorld 的实例对象，调用其中的 getMessage() 方法。

1. 首先会运行main()语句，Spring框架使用`ClassPathXmlApplicationContext()`首先创建一个容器。

   这个容器从`Beans.xml`中读取配置信息，并根据配置信息来创建bean(也就是对象)，每个bean有唯一的id。

2. 然后通过`context.getBean()`找到这个id的bean，获取对象的引用，强制转换为实例对象。

3. 通过对象的引用调用`getMessage()`方法来打印信息。

## 创建配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="helloWorld" class="com.wqcctgu.www.HelloWorld">
        <!-- class指的是class文件所在的路径 -->
        <property name="message" value="Hello World!" />
    </bean>
</beans>
```

Beans.xml 用于给不同的 Bean 分配唯一的 ID，并给相应的 Bean 属性赋值。例如，在以上代码中，我们可以在不影响其它类的情况下，给 message 变量赋值。

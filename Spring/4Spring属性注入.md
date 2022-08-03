Bean 属性注入，就是将属性注入到 Bean 中的过程，而这属性既可以普通属性，也可以是一个对象（Bean）。

Spring 主要通过以下 2 种方式实现属性注入：

- 构造函数注入(注入的参数需要与构造函数的参数相匹配)
- setter 注入（又称设值注入）

如果没有调用有参的构造函数，会调用无参构造函数执行(默认)。

## 构造函数注入

通过Bean的带参构造函数，以实现Bean的属性注入。

使用构造函数实现属性注入大致步骤如下：

1. 在 Bean 中添加一个有参构造函数，构造函数内的每一个参数代表一个需要注入的属性；
2. 在 Spring 的 XML 配置文件中，通过 <beans> 及其子元素 <bean> 对 Bean 进行定义；
3. 在 <bean> 元素内使用 <constructor-arg> 元素，对构造函数内的属性进行赋值，Bean 的构造函数内有多少参数，就需要使用多少个 <constructor-arg> 元素。

```java
//Grade.java
package com.wqcctgu.www;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Grade {
	private static final Log LOGGER = LogFactory.getLog(Grade.class);
	private Integer gradeId;
	private String gradeName;

	public Grade(Integer gradeId, String gradeName) {
		LOGGER.info("正在执行 Course 的有参构造方法，参数分别为：gradeId=" + gradeId + ",gradeName=" + gradeName);
		this.gradeId = gradeId;
		this.gradeName = gradeName;
	}

	@Override
	public String toString() {
		return "Grade{" + "gradeId=" + gradeId + ", gradeName='" + gradeName + '\'' + '}';
	}
}
```

```java
//Student.java
package com.wqcctgu.www;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Student {
	private static final Log LOGGER = LogFactory.getLog(Student.class);
	private int id;
	private String name;
	private Grade grade;

	public Student(int id, String name, Grade grade) {
		LOGGER.info("正在执行 Course 的有参构造方法，参数分别为：id=" + id + ",name=" + name + ",grade=" + grade);
		this.id = id;
		this.name = name;
		this.grade = grade;
	}

	@Override
	public String toString() {
		return "Student{" + "id=" + id + ", name='" + name + '\'' + ", grade=" + grade + '}';
	}
}
```

```java
//MainAPP.java
package net.biancheng.c;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    private static final Log LOGGER = LogFactory.getLog(MainApp.class);
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        //获取名为 student 的 Bean
        Student student = context.getBean("student", Student.class);
        //找到对应的id和class文件匹配信息
        //通过日志打印学生信息
        LOGGER.info(student.toString());
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
    <bean id="student" class="com.wqcctgu.www.Student">
        <constructor-arg name="id" value="2"></constructor-arg>
        <constructor-arg name="name" value="李四"></constructor-arg>
        <constructor-arg name="grade" ref="grade"></constructor-arg>
        <!--对应构造函数赋予属性值-->
    </bean>
    <bean id="grade" class="com.wqcctgu.www.Grade">
        <constructor-arg name="gradeId" value="4"></constructor-arg>
        <constructor-arg name="gradeName" value="四年级"></constructor-arg>
    </bean>
</beans>
```

## setter 注入

我们可以通过 Bean 的 setter 方法，将属性值注入到 Bean 的属性中。

在 Spring 实例化 Bean 的过程中，IoC 容器首先会调用默认的构造方法（无参构造方法）实例化 Bean（Java 对象），然后通过 Java 的反射机制调用这个 Bean 的 setXxx() 方法，将属性值注入到 Bean 中。

使用 setter 注入的方式进行属性注入，大致步骤如下：

1. 在 Bean 中提供一个默认的无参构造函数（在没有其他带参构造函数的情况下，可省略），并为所有需要注入的属性提供一个 setXxx() 方法；
2. 在 Spring 的 XML 配置文件中，使用 <beans> 及其子元素 <bean> 对 Bean 进行定义；
3. 在 <bean> 元素内使用 <property> 元素对各个属性进行赋值。

```java
//Grade.java
package com.wqcctgu.www;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Grade {
	private static final Log LOGGER = LogFactory.getLog(Grade.class);
	private Integer gradeId;
	private String gradeName;

	/**
	 * 无参构造函数 若该类中不存在其他的带参构造函数，则这个默认的无参构造函数可以省略
	 */
	public Grade() {
	}

	public void setGradeId(Integer gradeId) {
		LOGGER.info("正在执行 Grade 类的 setGradeId() 方法…… ");
		this.gradeId = gradeId;
	}

	public void setGradeName(String gradeName) {
		LOGGER.info("正在执行 Grade 类的 setGradeName() 方法…… ");
		this.gradeName = gradeName;
	}

	@Override
	public String toString() {
		return "Grade{" + "gradeId=" + gradeId + ", gradeName='" + gradeName + '\'' + '}';
	}
}
```

```java
//Student.java
package com.wqcctgu.www;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;

public class Student {
	private static final Log LOGGER = LogFactory.getLog(Student.class);
	private int id;
	private String name;
	private Grade grade;

	// 无参构造方法，在没有其他带参构造方法的情况下，可以省略
	public Student() {
	}

	// id 属性的 setter 方法
	public void setId(int id) {
		LOGGER.info("正在执行 Student 类的 setId() 方法…… ");
		this.id = id;
	}

	// name 属性的 setter 方法
	public void setName(String name) {
		LOGGER.info("正在执行 Student 类的 setName() 方法…… ");
		this.name = name;
	}

	public void setGrade(Grade grade) {
		LOGGER.info("正在执行 Student 类的 setGrade() 方法…… ");
		this.grade = grade;
	}

	@Override
	public String toString() {
		return "Student{" + "id=" + id + ", name='" + name + '\'' + ", grade=" + grade + '}';
	}
}
```

```java
//MainAPP.java
package com.wqcctgu.www;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MainApp {
	private static final Log LOGGER = LogFactory.getLog(MainApp.class);

	public static void main(String[] args) {
		// 获取 ApplicationContext 容器
		ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
		// 获取名为 student 的 Bean
		Student student = context.getBean("student", Student.class);
		// 通过日志打印学生信息
		LOGGER.info(student.toString());
	}
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="student" class="com.wqcctgu.www.Student">
        <!--使用 property 元素完成属性注入
            name: 类中的属性名称，例如 id,name
            value: 向属性注入的值 例如 学生的 id 为 1，name 为张三
        -->
        <property name="id" value="1"></property>
        <property name="name" value="张三"></property>
        <property name="grade" ref="grade"></property>
    </bean>
    <bean id="grade" class="com.wqcctgu.www.Grade">
        <property name="gradeId" value="3"></property>
        <property name="gradeName" value="三年级"></property>
    </bean>
</beans>
```

## 短命名空间注入

| 短命名空间 | 简化的 XML 配置                        | 说明                                     |
| ---------- | -------------------------------------- | ---------------------------------------- |
| p 命名空间 | <bean> 元素中嵌套的 <property> 元素    | 是 setter 方式属性注入的一种快捷实现方式 |
| c 命名空间 | <bean> 元素中嵌套的 <constructor> 元素 | 是构造函数属性注入的一种快捷实现方式     |

### p 命名空间注入

配置文件的<beans>元素导入以下xml约束

```xml
xmlns:p="http://www.springframework.org/schema/p"
```

在导入xml元素后，我们能通过以下格式：

>**<bean** id="Bean 唯一标志符" class="包名+类名" p:普通属性="普通属性值" p:对象属性-ref="对象的引用"**>**

使用 p 命名空间注入依赖时，必须注意以下 3 点：

- Java 类中必须有 setter 方法；
- Java 类中必须有无参构造器（类中不包含任何带参构造函数的情况，无参构造函数默认存在）；
- 在使用 p 命名空间实现属性注入前，XML 配置的 <beans> 元素内必须先导入 p 命名空间的 XML 约束。

### c命名空间注入

c 命名空间是构造函数注入的一种快捷实现方式。通过它，我们能够以 <bean> 属性的形式实现构造函数方式的属性注入，而不再使用嵌套的 <constructor-arg> 元素，以实现简化 Spring 的 XML 配置的目的。

```xml
xmlns:c="http://www.springframework.org/schema/c"
```

>**<bean** id="Bean 唯一标志符" class="包名+类名" c:普通属性="普通属性值" c:对象属性-ref="对象的引用"**>**

使用 c 命名空间注入依赖时，必须注意以下 2 点：

- Java 类中必须包含对应的带参构造器；
- 在使用 c 命名空间实现属性注入前，XML 配置的 <beans> 元素内必须先导入 c 命名空间的 XML 约束。

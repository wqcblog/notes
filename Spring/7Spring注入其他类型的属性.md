# Spring注入其他类型的属性

除了普通属性、对象属性（Bean）、集合等属性外，Spring 也能够将其他类型的属性注入到 Bean 中，例如 Null 值、字面量、复合物属性等。

## 注入NULL值

我们可以在 XML 配置文件中，通过 <null/> 元素将 Null 值注入到 Bean 中。

```java
//ExampleBean.java
package com.wqcctgu.www;
public class ExampleBean {
    private String propertyNull;
    public void setPropertyNull(String propertyNull) {
        this.propertyNull = propertyNull;
    }
    @Override
    public String toString() {
        return "ExampleBean{" +
                "propertyNull='" + propertyNull + '\'' +
                '}';
    }
}
```

```java
<!--Beans.xml-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="exampleBean" class="com.wqcctgu.www.ExampleBean">
        <!--使用null 标签注入 Null 值-->
        <property name="propertyNull">
            <null/>
        </property>
    </bean>
</beans>
```

```java
//MainApp.java
package com.wqcctgu.www;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    private static final Log LOGGER = LogFactory.getLog(MainApp.class);
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        //获取名为 exampleBean 的 Bean
        ExampleBean exampleBean = context.getBean("exampleBean", ExampleBean.class);
        //通过日志打印信息
        LOGGER.info(exampleBean.toString());
    }
}
```

## 注入空字符串

Spring 会将属性中空参数直接当作空字符串来处理。

```java
//ExampleBean.java
package com.wqcctgu.www;
public class ExampleBean {
    private String propertyNull;
    private String propertyEmpty;
    public void setPropertyEmpty(String propertyEmpty) {
        this.propertyEmpty = propertyEmpty;
    }
    public void setPropertyNull(String propertyNull) {
        this.propertyNull = propertyNull;
    }
    @Override
    public String toString() {
        return "ExampleBean{" +
                "propertyNull='" + propertyNull + '\'' +
                ", propertyEmpty='" + propertyEmpty + '\'' +
                '}';
    }
}
```

```xml
<!--Beans.xml-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="exampleBean" class="com.wqcctgu.www.ExampleBean">
        <!--使用null 标签注入 Null 值-->
        <property name="propertyNull">
            <null/>
        </property>
        <!--使用空参数注入空字符串-->
        <property name="propertyEmpty" value=""></property>
    </bean>
</beans>
```

```java
//MainApp.java
package com.wqcctgu.www;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    private static final Log LOGGER = LogFactory.getLog(MainApp.class);
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        //获取名为 exampleBean 的 Bean
        ExampleBean exampleBean = context.getBean("exampleBean", ExampleBean.class);
        //通过日志打印信息
        LOGGER.info(exampleBean.toString());
    }
}
```

## 注入字面量

我们知道，在 XML 配置中“<”、“>”、“&”等特殊字符是不能直接保存的，否则 XML 语法检查时就会报错。此时，我们可以通过以下两种方式将包含特殊符号的属性注入 Bean 中。

### 转义

在 XML 中，特殊符号必须进行转义才能保存进 XML 配置中，例如“&lt;”、“&gt;”、“&amp;”等。

在 XML 中，需要转义的字符如下表所示。

| 特殊字符 | 转义字符 |
| -------- | -------- |
| &        | `&amp;`  |
| <        | `&lt;`   |
| >        | `&gt;`   |
| ＂       | `&quot;` |
| ＇       | `&apos;` |

在转义过程中，需要注意以下几点：

- 转义序列字符之间不能有空格；
- 转义序列必须以“;”结束；
- 单独出现的“&”不会被认为是转义的开始；
- 区分大小写。

```java
//ExampleBean.java
package com.wqcctgu.www;

public class ExampleBean {
    //Null值
    private String propertyNull;
    //空字符串
    private String propertyEmpty;
    //包含特殊符号的字面量
    private String propertyLiteral;
    public void setPropertyEmpty(String propertyEmpty) {
        this.propertyEmpty = propertyEmpty;
    }
    public void setPropertyNull(String propertyNull) {
        this.propertyNull = propertyNull;
    }
    public void setPropertyLiteral(String propertyLiteral) {
        this.propertyLiteral = propertyLiteral;
    }
    @Override
    public String toString() {
        return "ExampleBean{" +
                "propertyNull='" + propertyNull + '\'' +
                ", propertyEmpty='" + propertyEmpty + '\'' +
                ", propertyLiteral='" + propertyLiteral + '\'' +
                '}';
    }
}
```

```xml
<!--Beans.xml-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="exampleBean" class="com.wqcctgu.www.ExampleBean">
        <!--使用null 标签注入 Null 值-->
        <property name="propertyNull">
            <null/>
        </property>
        <!--使用空参数注入空字符串-->
        <property name="propertyEmpty" value=""></property>
        <!--通过转义注入包含特殊符号的字面量-->
        <property name="propertyLiteral" value="&lt;测试&gt;"></property>
    </bean>
</beans>
```

```java
//MainApp.java
package com.wqcctgu.www;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    private static final Log LOGGER = LogFactory.getLog(MainApp.class);
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        //获取名为 exampleBean 的 Bean
        ExampleBean exampleBean = context.getBean("exampleBean", ExampleBean.class);
        //通过日志打印信息
        LOGGER.info(exampleBean.toString());
    }
}
```

### 使用短字符串

通过短字符串 <![CDATA[]]> 将包含特殊符号的属性值包裹起来，可以让 XML 解析器忽略对其中内容的解析，以属性原本的样子注入到 Bean 中。

使用短字符串 <![CDATA[]]> 需要注意以下几点：

-  此部分不能再包含”]]>”；
- 不允许嵌套使用；
- “]]>”中不能包含空格或者换行。

```xml
<!--Beans.xml-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="exampleBean" class="com.wqcctgu.www.ExampleBean">
        <!--使用null 标签注入 Null 值-->
        <property name="propertyNull">
            <null/>
        </property>
        <!--使用空参数注入空字符串-->
        <property name="propertyEmpty" value=""></property>
        <!--使用 <![CDATA[]]> 将包含特殊符号的字面量注入-->
        <property name="propertyLiteral">
            <value><![CDATA[<测试>]]></value>
        </property>
    </bean>
</beans>
```

### 级联属性赋值

我们可以在 <bean> 的 <property> 子元素中，为它所依赖的 Bean 的属性进行赋值，这就是所谓的“级联属性赋值”。

使用级联属性赋值时，需要注意以下 3点：

- Java 类中必须有 setter 方法；
- Java 类中必须有无参构造器（默认存在）；
- 依赖其他 Bean 的类中，必须提供一个它依赖的 Bean 的 getXxx() 方法。

```java
//ExampleBean.java
package com.wqcctgu.www;

public class ExampleBean {
    //Null值
    private String propertyNull;
    //空字符串
    private String propertyEmpty;
    //包含特殊符号的字面量
    private String propertyLiteral;
    //依赖的 Bean(对象属性)
    private DependBean dependBean;
    public void setPropertyEmpty(String propertyEmpty) {
        this.propertyEmpty = propertyEmpty;
    }
    public void setPropertyNull(String propertyNull) {
        this.propertyNull = propertyNull;
    }
    public void setPropertyLiteral(String propertyLiteral) {
        this.propertyLiteral = propertyLiteral;
    }
    public void setDependBean(DependBean dependBean) {
        this.dependBean = dependBean;
    }
    //使用级联属性赋值时，需提供一个依赖对象的 getXxx() 方法
    public DependBean getDependBean() {
        return dependBean;
    }
    @Override
    public String toString() {
        return "ExampleBean{" +
                "propertyNull='" + propertyNull + '\'' +
                ", propertyEmpty='" + propertyEmpty + '\'' +
                ", propertyLiteral='" + propertyLiteral + '\'' +
                ", dependBean=" + dependBean +
                '}';
    }
}
```

```xml
<!--Beans.xml-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="exampleBean" class="com.wqcctgu.www.ExampleBean">
        <!--使用null 标签注入 Null 值-->
        <property name="propertyNull">
            <null/>
        </property>
        <!--使用空参数注入空字符串-->
        <property name="propertyEmpty" value=""></property>
        <!--使用 <![CDATA[]]> 将包含特殊符号的字面量注入-->
        <property name="propertyLiteral">
            <value><![CDATA[<测试>]]></value>
        </property>
        <!--注入依赖的 Bean-->
        <property name="dependBean" ref="dependBean"></property>
        <!--ref为依赖的bean的id值,dependBean为Example中定义的一个类-->
        <!--级联属性赋值-->
        <property name="dependBean.dependProperty" value="级联属性赋值"></property>
        <!--这会覆盖依赖的Bean中对属性的赋值情况，可以通过.的方式指定类中的具体属性-->
    </bean>
    <!--对 ExampleBean 依赖的 Bean 进行定义-->
    <bean id="dependBean" class="com.wqcctgu.www.DependBean">
        <!--对属性进行赋值-->
        <property name="dependProperty" value="依赖 Bean 内部赋值"></property>
    </bean>
</beans>

<!--对比注入内部bean的用法(内部定义的只能用于该内部)，这个是对外部依赖的引用(重写)或者不重写直接使用外部依赖的值(详见自动装配)-->
```

```java
//DependBean.java
package com.wqcctgu.www;

public class DependBean {
    private String dependProperty;
    public void setDependProperty(String dependProperty) {
        this.dependProperty = dependProperty;
    }
    @Override
    public String toString() {
        return "DependBean{" +
                "dependProperty='" + dependProperty + '\'' +
                '}';
    }
}
```

```java
//MainApp.java
package com.wqcctgu.www;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;
public class MainApp {
    private static final Log LOGGER = LogFactory.getLog(MainApp.class);
    public static void main(String[] args) {
        //获取 ApplicationContext 容器
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        //获取名为 exampleBean 的 Bean
        ExampleBean exampleBean = context.getBean("exampleBean", ExampleBean.class);
        //通过日志打印信息
        LOGGER.info(exampleBean.toString());
    }
}
```


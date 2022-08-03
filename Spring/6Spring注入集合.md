# Spring注入集合

可以在 Bean 标签下的 <property> 元素中，使用以下元素配置 Java 集合类型的属性和参数，例如 List、Set、Map 以及 Properties 等。

| 标签    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| <list>  | 用于注入 list 类型的值，允许重复                             |
| <set>   | 用于注入 set 类型的值，不允许重复                            |
| <map>   | 用于注入 key-value 的集合，其中 key 和 value 都可以是任意类型 |
| <props> | 用于注入 key-value 的集合，其中 key 和 value 都是字符串类型  |

## 在集合中设置普通类型的值

```java
//JavaCollection.java
package com.wqcctgu.www;

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Set;

public class JavaCollection {
	// 1 数组类型属性
	private String[] courses;
	// 2 list 集合类型属性
	private List<String> list;
	// 3 map 集合类型属性
	private Map<String, String> maps;
	// 4 set 集合类型属性
	private Set<String> sets;

	public void setCourses(String[] courses) {
		this.courses = courses;
	}

	public void setList(List<String> list) {
		this.list = list;
	}

	public void setMaps(Map<String, String> maps) {
		this.maps = maps;
	}

	public void setSets(Set<String> sets) {
		this.sets = sets;
	}

	@Override
	public String toString() {
		return "JavaCollection{" + "courses=" + Arrays.toString(courses) + ", list=" + list + ", maps=" + maps
				+ ", sets=" + sets + '}';
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
		// 获取 ApplicationContext 容器
		ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
		// 获取名为 employee 的 Bean
		JavaCollection javaCollection = context.getBean("javaCollection", JavaCollection.class);
		// 通过日志打印员工信息
		LOGGER.info(javaCollection.toString());
	}
}
```

```xml
<!-- Beans.xml -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="javaCollection" class="com.wqcctgu.www.JavaCollection">
        <!--数组类型-->
        <property name="courses">
            <array>
                <value>Java</value>
                <value>PHP</value>
                <value>C 语言</value>
            </array>
        </property>
        <!--List 类型-->
        <property name="list">
            <list>
                <value>张三</value>
                <value>李四</value>
                <value>王五</value>
                <value>赵六</value>
            </list>
        </property>
        <!--Map 类型-->
        <property name="maps">
            <map>
                <entry key="JAVA" value="java"></entry>
                <entry key="PHP" value="php"></entry>
            </map>
        </property>
        <!--Set 类型-->
        <property name="sets">
            <set>
                <value>MySQL</value>
                <value>Redis</value>
            </set>
        </property>
    </bean>
</beans>
```

## 在集合中设置对象类型的值

```java
//Course.java
package com.wqcctgu.www;
import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
public class Course {
    private static final Log LOGGER = LogFactory.getLog(Course.class);
    //课程编号
    private Integer courseId;
    //课程名称
    private String courseName;
    public void setCourseId(Integer courseId) {
        this.courseId = courseId;
    }
    public void setCourseName(String courseName) {
        this.courseName = courseName;
    }
    @Override
    public String toString() {
        return "Course{" +
                "courseId=" + courseId +
                ", courseName='" + courseName + '\'' +
                '}';
    }
}
```

```java
//JavaCollection.java
package com.wqcctgu.www;
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.Set;
public class JavaCollection {
    //1 数组类型属性
    private Course[] courses;
    //2 list 集合类型属性
    private List<String> list;
    //3 map 集合类型属性
    private Map<String, String> maps;
    //4 set 集合类型属性
    private Set<String> sets;
    public void setCourses(Course[] courses) {
        this.courses = courses;
    }
    public void setList(List<String> list) {
        this.list = list;
    }
    public void setMaps(Map<String, String> maps) {
        this.maps = maps;
    }
    public void setSets(Set<String> sets) {
        this.sets = sets;
    }
    @Override
    public String toString() {
        return "JavaCollection{" +
                "courses=" + Arrays.toString(courses) +
                ", list=" + list +
                ", maps=" + maps +
                ", sets=" + sets +
                '}';
    }
}
```

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="course" class="net.biancheng.c.Course">
        <property name="courseId" value="1"></property>
        <property name="courseName" value="Java课程"></property>
    </bean>
    <bean id="course2" class="net.biancheng.c.Course">
        <property name="courseId" value="2"></property>
        <property name="courseName" value="PHP课程"></property>
    </bean>
    <bean id="course3" class="net.biancheng.c.Course">
        <property name="courseId" value="3"></property>
        <property name="courseName" value="C语言课程"></property>
    </bean>
    <bean id="javaCollection" class="net.biancheng.c.JavaCollection">
        <!--数组类型-->
        <property name="courses">
            <array>
                <ref bean="course"></ref>
                <ref bean="course2"></ref>
                <ref bean="course3"></ref>
            </array>
        </property>
        <!--List 类型-->
        <property name="list">
            <list>
                <value>张三</value>
                <value>李四</value>
                <value>王五</value>
                <value>赵六</value>
            </list>
        </property>
        <!--Map 类型-->
        <property name="maps">
            <map>
                <entry key="JAVA" value="java"></entry>
                <entry key="PHP" value="php"></entry>
            </map>
        </property>
        <!--Set 类型-->
        <property name="sets">
            <set>
                <value>MySQL</value>
                <value>Redis</value>
            </set>
        </property>
    </bean>
</beans>
```


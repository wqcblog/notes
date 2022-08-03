## Spring注入内部Bean

将定义在 <bean> 元素的 <property> 或 <constructor-arg> 元素内部的 Bean，称为“内部 Bean”。

### setter方式注入内部Bean

我们可以通过 setter 方式注入内部 Bean。此时，我们只需要在 <bean> 标签下的 <property> 元素中，再次使用 <bean> 元素对内部 Bean 进行定义，格式如下。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="outerBean" class="……">
        <property name="……" >
            <!-- 定义内部 Bean -->
            <bean class="……">
                <property name="……" value="……" ></property>
                ……
            </bean>
        </property>
    </bean>
</beans>
```

>注意：内部 Bean 都是匿名的，不需要指定 id 和 name 的。即使制定了，IoC 容器也不会将它作为区分 Bean 的标识符，反而会无视 Bean 的 Scope 标签。因此内部 Bean 几乎总是匿名的，且总会随着外部的 Bean 创建。内部 Bean 是无法被注入到它所在的 Bean 以外的任何其他 Bean 的。

```java
//Employee.java
package com.wqcctgu.www;

public class Employee {
	// 员工编号
	private String empNo;
	// 员工姓名
	private String empName;
	// 部门信息
	private Dept dept;

	public void setEmpNo(String empNo) {
		this.empNo = empNo;
	}

	public void setEmpName(String empName) {
		this.empName = empName;
	}

	public void setDept(Dept dept) {
		this.dept = dept;
	}

	@Override
	public String toString() {
		return "Employee{" + "empNo='" + empNo + '\'' + ", empName='" + empName + '\'' + ", dept=" + dept + '}';
	}
}
```

```java
//Dept.java
package com.wqcctgu.www;

public class Dept {
	// 部门编号
	private String deptNo;
	// 部门名称
	private String deptName;

	public void setDeptNo(String deptNo) {
		this.deptNo = deptNo;
	}

	public void setDeptName(String deptName) {
		this.deptName = deptName;
	}

	@Override
	public String toString() {
		return "Dept{" + "deptNo='" + deptNo + '\'' + ", deptName='" + deptName + '\'' + '}';
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
		Employee employee = context.getBean("employee", Employee.class);
		// 通过日志打印员工信息
		LOGGER.info(employee.toString());
	}
}
```

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="employee" class="com.wqcctgu.www.Employee">
        <property name="empNo" value="001"></property>
        <property name="empName" value="小王"></property>
        <property name="dept">
            <!--内部 Bean-->
            <bean class="com.wqcctgu.www.Dept">
                <property name="deptNo" value="004"></property>
                <property name="deptName" value="技术部"></property>
            </bean>
        </property>
    </bean>
</beans>
```

### 构造函数方式注入内部Bean

通过构造方法注入内部 Bean。此时，我们只需要在 <bean> 标签下的 <constructor-arg> 元素中，再次使用 <bean> 元素对内部 Bean 进行定义，格式如下：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="……" class="……">
        <constructor-arg name="……">
            <!--内部 Bean-->
            <bean class="……">
                <constructor-arg name="……" value="……"></constructor-arg>
                ……
            </bean>
        </constructor-arg>
    </bean>
</beans>
```


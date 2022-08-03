我们把Spring在Bean与Bean之间建立依赖关系的行为称为“装配”。

Spring 的 IOC 容器虽然功能强大，但它本身不过只是一个空壳而已，它自己并不能独自完成装配工作。需要我们主动将 Bean 放进去，并告诉它 Bean 和 Bean 之间的依赖关系，它才能按照我们的要求完成装配工作。

在前面的学习中，我们都是在 XML 配置中通过 <constructor-arg>和 <property> 中的 ref 属性，手动维护 Bean 与 Bean 之间的依赖关系的。

例如，一个部门（Dept）可以有多个员工（Employee），而一个员工只可能属于某一个部门，这种关联关系定义在 XML 配置的 Bean 定义中，形式如下。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <!--部门 Dept 的 Bean 定义-->
    <bean id="dept" class="com.wqcctgu.www.Dept"></bean>
    <!--雇员 Employee 的 Bean 定义-->
    <bean id="employee" class="com.wqcctgu.www.Employee">
        <!--通过 <property> 元素维护 Employee 和 Dept 的依赖关系-->
        <property name="dept" ref="dept"></property>
        <!--dept为依赖的级联元素的id值,name为属性名称-->
    </bean>
</beans>
```

对于只包含少量 Bean 的应用来说，这种方式已经足够满足我们的需求了。但随着应用的不断发展，容器中包含的 Bean 会越来越多，Bean 和 Bean 之间的依赖关系也越来越复杂，这就使得我们所编写的 XML 配置也越来越复杂，越来越繁琐。

我们知道，过于复杂的 XML 配置不但可读性差，而且编写起来极易出错，严重的降低了开发人员的开发效率。为了解决这一问题，Spring 框架还为我们提供了“自动装配”功能。

## Spring的自动装配

Spring 的自动装配功能可以让 Spring 容器依据某种规则（自动装配的规则，有五种），为指定的 Bean 从应用的上下文（AppplicationContext 容器）中查找它所依赖的 Bean，并自动建立 Bean 之间的依赖关系。而这一过程是在完全不使用任何 <constructor-arg>和 <property> 元素 ref 属性的情况下进行的。

Spring 的自动装配功能能够有效地简化 Spring 应用的 XML 配置，因此在配置数量相当多时采用自动装配降低工作量。

Spring 框架式默认不支持自动装配的，要想使用自动装配，则需要对 Spring XML 配置文件中 <bean> 元素的 autowire 属性进行设置。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <!--部门 Dept 的 Bean 定义-->
    <bean id="dept" class="com.wqcctgu.www.Dept"></bean>
   
    <!--雇员 Employee 的 Bean 定义,通过 autowire 属性设置自动装配的规则-->
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="byName">
    </bean>
</beans>
```

### 自动装配规则

Spring提供了5种自动装配规则，分别与autowire属性的五种取值相对应，具体说明如下：

| 属性值      | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| byName      | 按名称自动装配。  Spring 会根据的 Java 类中对象属性的名称，在整个应用的上下文 ApplicationContext（IoC 容器）中查找。若某个 Bean 的 id 或 name 属性值与这个对象属性的名称相同，则获取这个 Bean，并与当前的 Java 类 Bean 建立关联关系。 |
| byType      | 按类型自动装配。  Spring 会根据 Java 类中的对象属性的类型，在整个应用的上下文 ApplicationContext（IoC 容器）中查找。若某个 Bean 的 class 属性值与这个对象属性的类型相匹配，则获取这个 Bean，并与当前的 Java 类的 Bean 建立关联关系。 |
| constructor | 与 byType 模式相似，不同之处在与它应用于构造器参数（依赖项），如果在容器中没有找到与构造器参数类型一致的 Bean，那么将抛出异常。  其实就是根据构造器参数的数据类型，进行 byType 模式的自动装配。 |
| default     | 表示默认采用上一级元素 <beans> 设置的自动装配规则（default-autowire）进行装配。 |
| no          | 默认值，表示不使用自动装配，Bean 的依赖关系必须通过 <constructor-arg>和 <property> 元素的 ref 属性来定义。 |

```java
//Dept.java
package com.wqcctgu.www;

public class Dept {
    //部门编号
    private String deptNo;
    //部门名称
    private String deptName;
    public Dept() {
        System.out.println("正在执行 Dept 的无参构造方法>>>>");
    }
    public Dept(String deptNo, String deptName) {
        System.out.println("正在执行 Dept 的有参构造方法>>>>");
        this.deptNo = deptNo;
        this.deptName = deptName;
    }
    public void setDeptNo(String deptNo) {
        System.out.println("正在执行 Dept 的 setDeptNo 方法>>>>");
        this.deptNo = deptNo;
    }
    public void setDeptName(String deptName) {
        System.out.println("正在执行 Dept 的 setDeptName 方法>>>>");
        this.deptName = deptName;
    }
    public String getDeptNo() {
        return deptNo;
    }
    public String getDeptName() {
        return deptName;
    }
    @Override
    public String toString() {
        return "Dept{" +
                "deptNo='" + deptNo + '\'' +
                ", deptName='" + deptName + '\'' +
                '}';
    }
}
```

```java
//Employee.java
package com.wqcctgu.www;

public class Employee {
    //员工编号
    private String empNo;
    //员工姓名
    private String empName;
    //部门信息
    private Dept dept;
    public Employee() {
        System.out.println("正在执行 Employee 的无参构造方法>>>>");
    }
    public Employee(String empNo, String empName, Dept dept) {
        System.out.println("正在执行 Employee 的有参构造方法>>>>");
        this.empNo = empNo;
        this.empName = empName;
        this.dept = dept;
    }
    public void setEmpNo(String empNo) {
        System.out.println("正在执行 Employee 的 setEmpNo 方法>>>>");
        this.empNo = empNo;
    }
    public void setEmpName(String empName) {
        System.out.println("正在执行 Employee 的 setEmpName 方法>>>>");
        this.empName = empName;
    }
    public void setDept(Dept dept) {
        System.out.println("正在执行 Employee 的 setDept 方法>>>>");
        this.dept = dept;
    }
    public Dept getDept() {
        return dept;
    }
    public String getEmpNo() {
        return empNo;
    }
    public String getEmpName() {
        return empName;
    }
    @Override
    public String toString() {
        return "Employee{" +
                "empNo='" + empNo + '\'' +
                ", empName='" + empName + '\'' +
                ", dept=" + dept +
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
        ApplicationContext context = new ClassPathXmlApplicationContext("Beans.xml");
        Employee employee = context.getBean("employee", Employee.class);
        System.out.println(employee);
    }
}
```

#### 不使用自动装配

autowire="no" 表示不使用自动装配，此时我们必须通过 <bean> 元素的 <constructor-arg>和 <property> 元素的 ref 属性维护 Bean 的依赖关系。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd" default-autowire="constructor">
    <bean id="dept" class="com.wqcctgu.www.Dept">
        <property name="deptNo" value="1"></property>
        <property name="deptName" value="技术部"></property>
    </bean>
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="no">
        <property name="empNo" value="002"></property>
        <property name="empName" value="小郭"></property>
        <property name="dept" ref="dept"></property>
        <!--使用级联匹配，进行属性的注入-->
    </bean>
</beans>
```

>正在执行 Dept 的无参构造方法>>>>
>正在执行 Dept 的 setDeptNo 方法>>>>
>正在执行 Dept 的 setDeptName 方法>>>>
>正在执行 Employee 的无参构造方法>>>>
>正在执行 Employee 的 setEmpNo 方法>>>>
>正在执行 Employee 的 setEmpName 方法>>>>
>正在执行 Employee 的 setDept 方法>>>>
>Employee{empNo='002', empName='小郭', dept=Dept{deptNo='1', deptName='技术部'}}

#### 按名称自动装配

autowire="byName" 表示按属性名称自动装配，XML 文件中 Bean 的 id 或 name 必须与类中的属性名称相同。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd" >
    <bean id="dept" class="com.wqcctgu.www.Dept">
        <property name="deptNo" value="1"></property>
        <property name="deptName" value="技术部"></property>
    </bean>
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="byName">
        <property name="empNo" value="002"></property>
        <property name="empName" value="小郭"></property>
    </bean>
</beans>
```

>正在执行 Dept 的无参构造方法>>>>
>正在执行 Dept 的 setDeptNo 方法>>>>
>正在执行 Dept 的 setDeptName 方法>>>>
>正在执行 Employee 的无参构造方法>>>>
>正在执行 Employee 的 setEmpNo 方法>>>>
>正在执行 Employee 的 setEmpName 方法>>>>
>正在执行 Employee 的 setDept 方法>>>>
>Employee{empNo='002', empName='小郭', dept=Dept{deptNo='1', deptName='技术部'}}

如果id不同时，例如：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd" >
    <bean id="dept2" class="com.wqcctgu.www.Dept">
        <property name="deptNo" value="1"></property>
        <property name="deptName" value="技术部"></property>
    </bean>
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="byName">
        <property name="empNo" value="002"></property>
        <property name="empName" value="小郭"></property>
    </bean>
</beans>
```

>正在执行 Dept 的无参构造方法>>>>
>正在执行 Dept 的 setDeptNo 方法>>>>
>正在执行 Dept 的 setDeptName 方法>>>>
>正在执行 Employee 的无参构造方法>>>>
>正在执行 Employee 的 setEmpNo 方法>>>>
>正在执行 Employee 的 setEmpName 方法>>>>
>Employee{empNo='002', empName='小郭', dept=null}

#### 按类型自动装配

autowire="byType" 表示按类中对象属性数据类型进行自动装配。即使 XML 文件中 Bean 的 id 或 name 与类中的属性名不同，只要 Bean 的 class 属性值与类中的对象属性的类型相同，就可以完成自动装配。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd" >
    <bean id="dept2" class="com.wqcctgu.www.Dept">
        <property name="deptNo" value="1"></property>
        <property name="deptName" value="技术部"></property>
    </bean>
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="byType">
        <property name="empNo" value="002"></property>
        <property name="empName" value="小郭"></property>
    </bean>
</beans>
```

>正在执行 Dept 的无参构造方法>>>>
>正在执行 Dept 的 setDeptNo 方法>>>>
>正在执行 Dept 的 setDeptName 方法>>>>
>正在执行 Employee 的无参构造方法>>>>
>正在执行 Employee 的 setEmpNo 方法>>>>
>正在执行 Employee 的 setEmpName 方法>>>>
>正在执行 Employee 的 setDept 方法>>>>
>Employee{empNo='002', empName='小郭', dept=Dept{deptNo='1', deptName='技术部'}}

如果存在多个相同类型的Bean，则注入失败，并且引发异常。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="dept" class="com.wqcctgu.www.Dept">
        <property name="deptNo" value="02"></property>
        <property name="deptName" value="运维部"></property>
    </bean>
    <bean id="dept2" class="com.wqcctgu.www.Dept">
        <property name="deptNo" value="1"></property>
        <property name="deptName" value="技术部"></property>
    </bean>
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="byType">
        <property name="empNo" value="002"></property>
        <property name="empName" value="小郭"></property>
    </bean>
</beans>
```

#### 构造函数自动装配

autowire="constructor" 表示仅按照 Java 类中构造函数(仅调用constructor的注入)进行自动装配。

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
    <bean id="dept2" class="com.wqcctgu.www.Dept">
        <constructor-arg name="deptNo" value="1"></constructor-arg>
        <constructor-arg name="deptName" value="技术部"></constructor-arg>
    </bean>
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="constructor">
        <constructor-arg name="empNo" value="002"></constructor-arg>
        <constructor-arg name="empName" value="小郭"></constructor-arg>
    </bean>
</beans>
```

#### 默认的自动装配模式

默认采用上一级标签 <beans> 设置的自动装配规则（default-autowire）进行装配，Beans.xml 中的配置内容如下。 

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
   http://www.springframework.org/schema/beans/spring-beans-3.0.xsd" default-autowire="byType">
    <bean id="dept2" class="com.wqcctgu.www.Dept">
        <property name="deptNo" value="1"></property>
        <property name="deptName" value="技术部"></property>
    </bean>
    <bean id="employee" class="com.wqcctgu.www.Employee" autowire="default">
        <property name="empNo" value="002"></property>
        <property name="empName" value="小郭"></property>
    </bean>
</beans>
<!--这里使用上一级标签设置的byType方式进行装配-->
```

>正在执行 Dept 的无参构造方法>>>>
>正在执行 Dept 的 setDeptNo 方法>>>>
>正在执行 Dept 的 setDeptName 方法>>>>
>正在执行 Employee 的无参构造方法>>>>
>正在执行 Employee 的 setEmpNo 方法>>>>
>正在执行 Employee 的 setEmpName 方法>>>>
>正在执行 Employee 的 setDept 方法>>>>
>Employee{empNo='002', empName='小郭', dept=Dept{deptNo='1', deptName='技术部'}}

## 执行机制

Java 程序的运行必须经过编写、编译和运行 3 个步骤。

1. 编写：是指在 Java 开发环境中进行程序代码的输入，最终形成后缀名为 .java 的 Java 源文件。
2. 编译：是指使用 Java 编译器对源文件进行错误排査的过程，编译后将生成后缀名为 .class 的字节码文件，不像C语言那样生成可执行文件。
3. 运行：是指使用 Java 解释器将字节码文件翻译成机器代码，执行并显示结果。
   Java 程序运行流程如图 1 所示。



![Java程序运行流程](https://raw.githubusercontent.com/wqcblog/picgo-image/master/img/202110121408038.png)
图 1 Java程序运行流程


字节码文件是一种和任何具体机器环境及操作系统环境无关的中间代码。它是一种二进制文件，是 Java 源文件由 Java 编译器编译后生成的目标代码文件。编程人员和计算机都无法直接读懂字节码文件，它必须由专用的 Java 解释器来解释执行，因此 Java 是一种在编译基础上进行解释运行的语言。

Java 解释器负责将字节码文件翻译成具体硬件环境和操作系统平台下的机器代码，以便执行。因此 Java 程序不能直接运行在现有的操作系统平台上，它必须运行在被称为 Java 虚拟机的软件平台之上。

Java 虚拟机（JVM）是运行 Java 程序的软件环境，Java 解释器是 Java 虚拟机的一部分。在运行 Java 程序时，首先会启动 JVM，然后由它来负责解释执行 Java 的字节码程序，并且 Java 字节码程序只能运行于 JVM 之上。这样利用 JVM 就可以把 Java 字节码程序和具体的硬件平台以及操作系统环境分隔开来，只要在不同的计算机上安装了针对特定平台的 JVM，Java 程序就可以运行，而不用考虑当前具体的硬件平台及操作系统环境，也不用考虑字节码文件是在何种平台上生成的。

JVM 把这种不同软、硬件平台的具体差别隐藏起来，从而实现了真正的二进制代码级的跨平台移植。JVM 是 Java 平台架构的基础，Java 的跨平台特性正是通过在 JVM 中运行 Java 程序实现的。Java 的这种运行机制可以通过图 2 来说明。



![JVM工作方式](https://raw.githubusercontent.com/wqcblog/picgo-image/master/img/202110121408736.png)
图 2 JVM工作方式



Java 语言这种“一次编写，到处运行”的方式，有效地解决了目前大多数高级程序设计语言需要针对不同系统来编译产生不同机器代码的问题，即硬件环境和操作平台的异构问题，大大降低了程序开发、维护和管理的开销。

提示：Java 程序通过 JVM 可以实现跨平台特性，但 JVM 是不跨平台的。也就是说，不同操作系统之上的 JVM 是不同的，Windows 平台之上的 JVM 不能用在 Linux 平台，反之亦然。

## JVM、JRE和JDK

- JDK（Java Development Kid，Java 开发开源工具包），是针对 Java 开发人员的产品，是整个 Java 的核心，包括了 Java 运行环境 JRE、Java 工具和 Java 基础类库。
- JRE（Java Runtime Environment，Java 运行环境）是运行 JAVA 程序所必须的环境的集合，包含 JVM 标准实现及 Java 核心类库。
- JVM（Java Virtual Machine，Java 虚拟机）是整个 Java 实现跨平台的最核心的部分，能够运行以 Java 语言写作的软件程序。


所以说大家看出来三者的关系了吗？其实如下图所示：

![img](https://raw.githubusercontent.com/wqcblog/picgo-image/master/img/202110121413786.jpeg)


![img](https://raw.githubusercontent.com/wqcblog/picgo-image/master/img/202110121413778.png)


由图中可以看出以下几点：

- JDK=JRE+多种Java开发工具
- JRE=JVM+各种类库
- 这三者的关系是一层层的嵌套关系。JDK>JRE>JVM

## C++与java的区别

1. C++ 支持指针，而 Java 没有指针的概念。
2. C++ 支持多继承，而 Java 不支持多重继承，但允许一个类实现多个接口。
3. Java 是完全面向对象的语言，并且还取消了 C/C++ 中的结构和联合，使编译程序更加简洁
4. Java 自动进行无用内存回收操作，不再需要程序员进行手动删除，而 C++ 中必须由程序释放内存资源，这就增加了程序员的负担。
5. Java 不支持操作符重载，操作符重载则被认为是 C++ 的突出特征。
6. Java 允许预处理，但不支持预处理器功能，所以为了实现预处理，它提供了引入语句（import），但它与 C++ 预处理器的功能类似。
7. Java 不支持缺省参数函数，而 C++ 支持 。
8. C 和 C++ 不支持字符串变量，在 C 和 C++ 程序中使用“Null”终止符代表字符串的结束。在 Java 中字符串是用类对象（String 和 StringBuffer）来实现的
9. goto 语句是 C 和 C++ 的“遗物”，Java 不提供 goto 语句，虽然 Java 指定 goto 作为关键字，但不支持它的使用，这使程序更简洁易读。
10. Java 不支持 C++ 中的自动强制类型转换，如果需要，必须由程序显式进行强制类型转换。

## 主流javaweb开发框架

### Spring 框架

Spring 框架是一个轻量级的框架，渗透了 Java EE 技术的方方面面。Spring 框架是由于软件开发的复杂性而创建的，是一个开源框架。

Spring 框架的用途不仅限于服务器端的开发，从简单性、可测试性和松耦合性角度而言，绝大部分 Java 应用都可以从 Spring 框架中受益。

对 Spring 框架的几点说明：

- 目的：解决企业应用开发的复杂性。
- 目标：Java EE 技术更容易使用，并促进良好编程习惯的养成。
- 功能：使用基本的 JavaBean 代替 EJB，并提供更多的企业应用功能。
- 范围：任何 Java 应用。


Spring 框架是一个轻量级控制反转（IoC）和面向切面（AOP）的容器框架，它主要作为依赖注入容器和 AOP 实现存在，还提供了声明式事务、对 DAO 层的支持等简化开发的功能。

Spring 框架可以很方便地与 Spring MVC、Struts 2、MyBatis、Hibernate 等框架集成，其中大名鼎鼎的 SSM 集成框架指的就是基于 Spring MVC + Spring + MyBatis 的技术框架，使用这个集成框架能使应用程序更加健壮、稳固、轻巧和优雅，这也是当前流行的 Java Web 技术框架。

Spring 框架官网：https://spring.io/

### Spring MVC 框架

Spring MVC 框架属于 SpringFrameWork 的后续产品，已经融合在 Spring Web Flow 中，是结构清晰的 MVC Model2 的实现。

Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块，并且拥有高度的可配置性，支持多种视图技术。它还可以进行定制化开发，使用相当灵活。

此外，Spring 框架整合 Spring MVC 框架是无缝集成，这是一个高性能的架构模式，已越来越广泛地应用于互联网应用的开发中。当使用 Spring 框架进行 Web 开发时，可以选择 Spring MVC 框架或集成其他 MVC 的开发框架，如 Struts 1（现在一般不用）、Struts 2（一般老项目使用）等。

### MyBatis 框架

MyBatis 框架是一个优秀的数据持久层框架，可在实体类和 SQL 语句之间建立映射关系，是一种半自动化的 ORM 实现。

Mybatis 的封装性要低于 Hibernate 框架，且性能优异、简单易学，因此应用较为广泛。

MyBatis 框架本是 Apache 的一个开源项目 iBatis，2010 年，这个项目由 Apache software foundation 迁移到 Google code，并且改名为“MyBatis”；2013 年 11 月它迁移到 Github。

“iBatis”一词来源于“internet”和“abatis”的组合，它是一个基于 Java 的持久层框架，其框架包括 SQL Maps 和 Data Access Objects（DAOs）。

MyBatis 3中文文档：https://mybatis.org/mybatis-3/zh/

### Hibernate 框架

Hibernate 框架不仅是一个优秀的持久化框架，也是一个开放源代码的对象关系映射框架。它对 JDBC 进行了轻量级的对象封装，将 POJO 与数据库表建立映射关系，形成一个全自动的 ORM 框架。

Hibernate 框架可以自动生成 SQL 语句，且自动执行，使 Java 程序员可以随心所欲地使用对象编程思维来操纵数据库。

Hibernate 框架还可以应用在任何使用 JDBC 的场合：

- 可以在 Java 的客户端程序使用；
- 也可以在 Servlet/JSP 的 Web 应用中使用；
- 最具革命意义的是，Hibernate 框架可以在应用 EJB 的 Jave EE 架构中取代 CMP，以完成数据持久化的重任。


Hibernate 框架已经成为当前主流的数据库持久化框架，并被广泛应用。

Hibernate 框架官网：http://hibernate.org/

### Struts 2 框架

Struts 2 框架以 WebWork 的优秀设计思想为核心，吸收 Struts 框架的部分优点，提供了一个更加简洁的基于 MVC 设计模式实现的 Web 应用程序框架，它本质上相当于一个 Servlet。

在 MVC 设计模式中，Struts 2 框架作为控制器（Controller）来建立模型与视图的数据交互。

Struts 2 框架是 Struts 的下一代产品，是在 Struts 1 和 WebWork 技术的基础上进行合并的创新。它采用拦截器的机制来处理用户的请求，可使业务逻辑控制器与 Servlet API 完全脱离开，所以也可以理解是 WebWork 的更新产品。

Struts 2 框架充分利用了其他 MVC 框架的经验和教训，使整个框架更加清晰和灵活。

Struts 框架官网：https://struts.apache.org/
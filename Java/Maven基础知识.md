## 基础设置

POM（Project Object Model，项目对象模型）是 Maven 的基本组件，它是以 xml 文件的形式存放在项目的根目录下，名称为 pom.xml。

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>net.biancheng.www</groupId>
    <artifactId>maven</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging> 
    <!--Web app的打包方式为war-->
</project>
```

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/07/20220729_1659084296.png" alt="image-20220729164455938" style="zoom:67%;" />

目录及文件说明：

- helloMaven：项目名，包含 src 文件夹和 pom.xml。
- src/main/java：用于存放项目的 Java 文件。
- src/main/resources：用于存放项目资源文件。
- src/test/java：用于存放所有测试 Java 文件，如 JUnit 测试类。
- src/test/resources ：用于存放测试资源文件。
- target：项目输出位置，用于存放编译后的文件。
- pom.xml：Maven 项目核心配置文件。

## 依赖

```xml
 <dependencies>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>servlet-api</artifactId>
            <version>2.5</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
```

dependencies 元素可以包含一个或者多个 dependency 子元素，用以声明一个或者多个项目依赖，每个依赖都可以包含以下元素：

- groupId、artifactId 和 version：依赖的基本坐标。
- type：依赖的类型，对应于项目坐标定义的 packaging。大部分情况下，该元素不必声明，其默认值是 jar。
- scope：依赖的范围。
- optional：标记依赖是否可选。
- exclusions：用来排除传递性依赖。

## 仓库

```xml
<localRepository>D:/myRepository/repository</localRepository>    
<repositories>
        <repository>
            <id>lib1</id>
            <url>http://download.bianchengbang.org/maven2/lib1</url>
        </repository>
        <repository>
            <id>lib2</id>
            <url>http://download.bianchengbang.org/maven2/lib2</url>
        </repository>
    </repositories>
```

依次从本地仓库到远程仓库搜索，镜像仓库是指对默认远程仓库的镜像。

## 插件

| 插件类型          | 描述                                                       |
| ----------------- | ---------------------------------------------------------- |
| Build plugins     | 在项目构建过程中执行，在 pom.xml 中的 build 元素中配置     |
| Reporting plugins | 在网站生成过程中执行，在 pom.xml 中的 reporting 元素中配置 |

插件执行语法：

```
mvn [插件名]:[目标名]
mvn compiler:compile
```

生命周期与插件内置的绑定：

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/07/20220729_1659064732.png" alt="image-20220729111851360" style="zoom: 67%;" />

常见的命令：

mvn clean，mvn compile，mvn test，mvn package，mvn install 

自定义绑定：

```xml
<project>
...
    <build>
        <plugins>
            <!-- 绑定插件 maven-antrun-plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-antrun-plugin</artifactId>
                <version>1.8</version>
                <executions>
                    <execution>
                        <!--自定义 id -->
                        <id>zidingyiclean</id>
                        <!--插件目标绑定的构建阶段 -->
                        <phase>clean</phase>
                        <!--插件目标 -->
                        <goals>
                            <goal>run</goal>
                        </goals>
                        <!--配置 -->
                        <configuration>
                            <!-- 执行的任务 -->
                            <tasks>
                                <!--自定义文本信息 -->
                                <echo>清理阶段</echo>
                            </tasks>
                        </configuration>
                    </execution>               
                </executions>
            </plugin>
        </plugins>
    </build>
...
</project>
```

## 导入本地jar

```xml
 <dependency>
            <groupId>net.biancheng.www</groupId>
            <artifactId>helloMaven</artifactId>
             <!--依赖范围-->
            <scope>system</scope>
            <version>1.0-SNAPSHOT</version>
            <!--依赖所在位置-->
            <systemPath>D:\maven\helloMaven\target\helloMaven-1.0-SNAPSHOT.jar</systemPath>
        </dependency>
```

在以上配置中，除了依赖的坐标信息外，外部依赖还使用了 scope 和 systemPath 两个元素。

- scope 表示依赖范围，这里取值必须是 system，即系统。
- systemPath 表示依赖的本地构件的位置。

## 生成site

POM 中可以包含各种项目信息，例如：项目描述、SCM 地址、许可证信息，开发者信息等。用户可以使用 Maven 提供的 maven-site-plugin 插件让 Maven 生成一个 Web 站点， 以站点的形式发布以上信息。

```xml
    <build>
        <plugins>
            <!--添加site 插件-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-site-plugin</artifactId>
                <version>3.7.1</version>
            </plugin>
        </plugins>
    </build>
```

```
mvn site
```

## 传递依赖

传递依赖写明坐标即可准确定位，但编译前需要对依赖的模块先install。

Maven 针对这些情况提供了如下功能进行处理。

- 依赖范围（Dependency scope）
- 依赖调解（Dependency mediation）
- 可选依赖（Optional dependencies）
- 排除依赖（Excluded dependencies）
- 依赖管理（Dependency management）

Maven 具有以下 6种常见的**依赖范围**(scope)，如下表所示。

| 依赖范围 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| compile  | 编译依赖范围，scope 元素的缺省值。使用此依赖范围的 Maven 依赖，对于三种 classpath 均有效，即该 Maven 依赖在上述三种 classpath 均会被引入。例如，log4j 在编译、测试、运行过程都是必须的。 |
| test     | 测试依赖范围。使用此依赖范围的 Maven 依赖，只对测试 classpath 有效。例如，Junit 依赖只有在测试阶段才需要。 |
| provided | 已提供依赖范围。使用此依赖范围的 Maven 依赖，只对编译 classpath 和测试 classpath 有效。例如，servlet-api 依赖对于编译、测试阶段而言是需要的，但是运行阶段，由于外部容器已经提供，故不需要 Maven 重复引入该依赖。 |
| runtime  | 运行时依赖范围。使用此依赖范围的 Maven 依赖，只对测试 classpath、运行 classpath 有效。例如，JDBC 驱动实现依赖，其在编译时只需 JDK 提供的 JDBC 接口即可，只有测试、运行阶段才需要实现了 JDBC 接口的驱动。 |
| system   | 系统依赖范围，其效果与 provided 的依赖范围一致。其用于添加非 Maven 仓库的本地依赖，通过依赖元素 dependency 中的 systemPath 元素指定本地依赖的路径。鉴于使用其会导致项目的可移植性降低，一般不推荐使用。 |
| import   | 导入依赖范围，该依赖范围只能与 dependencyManagement 元素配合使用，其功能是将目标 pom.xml 文件中 dependencyManagement 的配置导入合并到当前 pom.xml 的 dependencyManagement 中。 |

依赖范围与三种 classpath 的关系一览表，如下所示。

| 依赖范围 | 编译 classpath | 测试 classpath | 运行 classpath | 例子                    |
| -------- | -------------- | -------------- | -------------- | ----------------------- |
| compile  | √              | √              | √              | log4j                   |
| test     | -              | √              | -              | junit                   |
| provided | √              | √              | -              | servlet-api             |
| runtime  | -              | √              | √              | JDBC-driver             |
| system   | √              | √              | -              | 非 Maven 仓库的本地依赖 |

**依赖传递**

项目 A 依赖于项目 B，B 又依赖于项目 C，此时我们可以将 A 对于 B 的依赖称之为第一直接依赖，B 对于 C 的依赖称之为第二直接依赖。

注：上表中，左边第一列表示第一直接依赖的依赖范围，上边第一行表示第二直接依赖的依赖范围。交叉部分的单元格的取值为传递性依赖的依赖范围，若交叉单元格取值为“-”，则表示该传递性依赖不能被传递。

|          | compile  | test | provided | runtime  |
| -------- | -------- | ---- | -------- | -------- |
| compile  | compile  | -    | -        | runtime  |
| test     | test     | -    | -        | test     |
| provided | provided | -    | provided | provided |
| runtime  | runtime  | -    | -        | runtime  |

**依赖调节**

当一个间接依赖存在多条引入路径时，为了避免出现依赖重复的问题，Maven 通过依赖调节来确定间接依赖的引入路径。

依赖调节遵循以下两条原则：

1. 引入路径短者优先
2. 先声明者优先

**排除依赖**

假设存在这样的依赖关系，A 依赖于 B，B 依赖于 X，B 又依赖于 Y。B 实现了两个特性，其中一个特性依赖于 X，另一个特性依赖于 Y，且两个特性是互斥的关系，用户无法同时使用两个特性，所以 A 需要排除 X，此时就可以在 A 中将间接依赖 X 排除。

```xml
<!--在A中设置-->
<dependency>
            <groupId>net.biancheng.www</groupId>
            <artifactId>B</artifactId>
            <version>1.0-SNAPSHOT</version>
            <exclusions>
                <!-- 设置排除 -->
                <!-- B中optional必须设置为 false -->
                <exclusion>
                    <!--设置具体排除-->
                    <groupId>net.biancheng.www</groupId>
                    <artifactId>X</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
```

**可选依赖**

与上文的应用场景相同，也是 A 希望排除间接依赖 X，我们还可以在 B 中将 X 设置为可选依赖。

```xml
<!--在B中设置-->
       <dependency>
            <groupId>net.biancheng.www</groupId>
            <artifactId>X</artifactId>
            <version>1.0-SNAPSHOT</version>
            <!--设置可选依赖  -->
            <optional>true</optional>
        </dependency>
```

关于 optional 元素及可选依赖说明如下：

- 可选依赖用来控制当前依赖是否向下传递成为间接依赖；
- optional 默认值为 false，表示可以向下传递称为间接依赖；
- 若 optional 元素取值为 true，则表示当前依赖不能向下传递成为间接依赖。

## 继承

其他模块的 POM 可通过继承父模块的 POM 来获得对相关依赖的声明。对于父模块而言，其目的是为了消除子模块 POM 中的重复配置，其中不包含有任何实际代码，因此父模块 POM 的打包类型（packaging）必须是 pom。

```xml
<!--父模块-->
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>net.biancheng.www</groupId>
    <artifactId>Root</artifactId>
    <version>1.0</version>
    <!--定义的父类 POM 打包类型使pom  -->
    <packaging>pom</packaging>
   
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.9</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

```xml
<!--子模块-->
    <parent>
        <groupId>net.biancheng.www</groupId>
        <artifactId>Root</artifactId>
        <version>1.0</version>
        <relativePath>../Root/pom.xml</relativePath>
    </parent>
    <dependencies>
        <dependency>
            <artifactId>junit</artifactId>
        </dependency>
    </dependencies>
```
parent元素：


| 元素         | 描述                                                         | 是否必需 |
| ------------ | ------------------------------------------------------------ | -------- |
| groupId      | 父模块的项目组 id。                                          | 是       |
| artifactId   | 父模块 id。                                                  | 是       |
| version      | 父模块版本。                                                 | 是       |
| relativePath | 父模块 POM 的相对路径，默认值为 ../pom.xml。 项目构建时，Maven 会先根据 relativePath 查找父模块 POM，如果找不到，再从本地仓库或远程仓库中查找。 | 否       |

例如，子模块的 POM 中，当前模块的 groupId 和 version 元素可以省略，但这并不意味着当前模块没有 groupId 和 version，子模块会隐式的从父模块中继承这两个元素，即由父模块控制子模块的公司组织 id 以及版本，这样可以简化 POM 的配置。

## 依赖管理

Maven 父模块可以通过 dependencyManagement 元素对依赖进行管理，它具有以下 2 大特性：

- 在该元素下声明的依赖不会实际引入到模块中，只有在 dependencies 元素下同样声明了该依赖，才会引入到模块中。
- 该元素能够约束 dependencies 下依赖的使用，即 dependencies 声明的依赖若未指定版本，则使用 dependencyManagement 中指定的版本，否则将覆盖 dependencyManagement 中的版本。

**依赖管理的传递**

```xml
    <!--定义依赖管理-->
    <dependencyManagement>
        <dependencies>
            <!--导入依赖管理配置-->
            <dependency>
                <groupId>net.biancheng.www</groupId>
                <artifactId>Root</artifactId>
                <version>1.0</version>
                <!--依赖范围为 import-->
                <scope>import</scope>
                <!--类型一般为pom-->
                <type>pom</type>
            </dependency>
        </dependencies>
    </dependencyManagement>

```

## 定义变量

```xml
    <properties>
        <!-- 定义一些 maven 变量 -->
        <log4j.version>1.2.17</log4j.version>
        <junit.version>4.9</junit.version>
    </properties>
```

可以通过${变量名}直接使用。

## 资源配置

```xml
<!--可以扫描对应目录，对目录下的资源文件进行扫描，替换变量中的值-->
<resources>
    <resource>
        <directory>${project.basedir}/src/main/resources</directory>
        <filtering>true</filtering>
    </resource>
</resources>
```

## 聚合

聚合模块的打包方式（packaging）也是 pom，用户可以在其 POM 中通过 modules 下的 module 子元素来添加需要聚合的模块的目录路径。

```xml
    <!--添加需要聚合的模块-->
    <modules>
        <module>../App-Core-lib</module>
        <module>../App-Data-lib</module>
        <module>../App-UI-WAR</module>
    </modules>
```


Maven 的继承和聚合的目的不同，继承的目的是为了消除 POM 中的重复配置，聚合的目的是为了方便快速的构建项目。

对于继承中的父模块来说，它跟本不知道那些模块继承了它，但子模块都知道自己的父模块是谁。

对于聚合模块来说，它知道哪些模块被聚合了，但那些被聚合的模块根本不知道聚合模块的存在。

两者在结构和形式上还是有一定的共同点的，最直观的就是两者的打包方式都是 pom，两者除了 POM 外都没有实际的代码内容。

## pluginManagement

在 pluginManagement 元素中可以声明插件及插件配置，但不会发生实际的插件调用行为，只有在 POM 中配置了真正的 plugin 元素，且其 groupId 和 artifactId 与 pluginManagement 元素中配置的插件匹配时，pluginManagement 元素的配置才会影响到实际的插件行为。

在父模块中使用 pluginManagement 元素对插件版本进行统一声明，我们甚至可以将项目中所有插件的版本信息都在父模块的 POM 中声明，子模块中不再配置任何的版本信息，这样不仅可以统一项目的插件版本，还可以避免出现版本冲突或插件不稳定等问题。。

```xml
<!--父模块-->
<build>
        <pluginManagement>
            <plugins>
                <!--声明插件-->
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-source-plugin</artifactId>
                    <version>3.2.1</version>
                    <executions>
                        <!--将 jar-no-fork 目标绑定到 verify 阶段-->
                        <execution>
                            <id>www.biancheng.net</id>
                            <phase>verify</phase>
                            <goals>
                                <goal>
                                    jar-no-fork
                                </goal>
                            </goals>
                        </execution>
                    </executions>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
```

```xml
<!--子模块-->  
<plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-source-plugin</artifactId>
            </plugin>
        </plugins>
```

## Maven Profile

Profile 配置可以分为 3 个类型，它们的作用范围也各不相同。

| 类型        | 位置                                                | 有效范围                          |
| ----------- | --------------------------------------------------- | --------------------------------- |
| Per Project | Maven 项目的 pom.xml 中                             | 只对当前项目有效                  |
| Per User    | 用户主目录（％USER_HOME％）/.m2/settings.xml 中     | 对本机上该用户所有 Maven 项目有效 |
| Global      | Maven 安装目录（%MAVEN_HOME%）/conf/settings.xml 中 | 对本机上所有 Maven 项目有效       |

在 setting.xml 中声明的 Profile 是无法保证能够随着 pom.xml 一起被分发的，因此 Maven 不允许用户在该类型的 Profile 修改或增加依赖或插件等配置信息，它只能声明以下范围较为宽泛的元素。

- repositories：仓库配置。
- pluginRepositories：插件仓库配置。
- properties：键值对，该键值对可以在 pom.xml 中使用。

Profile 可以通过以下 6 种方式激活：

- 命令行激活
- settings.xml 文件显示激活
- 系统属性激活
- 操作系统环境激活
- 文件存在与否激活
- 默认激活

```xml
<!--命令行激活-->
<profiles>
        <!--test 环境配置  -->
        <profile>
            <id>test1</id>
            <activation>
                <property>
                    <name>env</name>
                    <value>test</value>
                </property>
            </activation>
            <build>
                <plugins>
                    <plugin>
                       略
                    </plugin>
                </plugins>
            </build>
        </profile>
        <!-- 默认环境配置 -->
        <profile>
            <id>test2</id>
            <build>
                <plugins>
                    <plugin>
                      略
                    </plugin>
                </plugins>
            </build>
        </profile>
</profiles>
```

```
mvn clean install -Ptest1,test2
```

```xml
<!--setting.xml中设置-->
<activeProfiles>
    <activeProfile>test1</activeProfile>
</activeProfiles>
```

```
mvn clean test
```

```xml
<!--系统属性激活-->
<!--id 为 prod 的 profile 元素中添加以下配置，表示当系统属性 user 存在，且值等于 prod 时，自动激活该 Profile。-->
<activation>
      <property>
        <name>user</name>
        <value>prod</value>
    </property>
</activation>
```

```
mvn clean test -Duser=prod
```

```xml
<!--操作系统环境激活，同上-->
<activation>
    <os>
        <name>Windows 10</name>
        <family>Windows</family>
        <arch>amd64</arch>
        <version>10.0</version>
    </os>
</activation>
```

```
mvn clean test
```

```xml
<!--文件存在与否激活-->
<activation>
        <file>
            <exists>./src/main/resources/env.prod.properties</exists>
            <missing>./src/main/resources/env.test.properties</missing>
        </file>
</activation>
```

```
mvn clean test 
```

```xml
<!--默认指定激活-->
<activation>
    <activeByDefault>true</activeByDefault>
</activation>
```

```
mvn clean test
```


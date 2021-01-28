# Maven

## 一、Eclipse整合Maven

> Window --- > Preferences --- > Maven

<img src="D:\user\notes\picture\Maven07.png">

### 1.1 Installations

<img src="D:\user\notes\picture\Maven08.png">

### 1.2 User Settings

<img src="D:\user\notes\picture\Maven09.png">

## 二、Eclipse创建Maven工程的两种方式

### 创建一个Maven工程

> new --- > project --- > Maven Project

### 1.不跳过骨架

<img src="D:\user\notes\picture\Maven01.png">

### 2.目录结构

<img src="D:\user\notes\picture\Maven03.png">

### 3.跳过骨架

<img src="D:\user\notes\picture\Maven05.png">

### 4.选择骨架

<img src="D:\user\notes\picture\Maven02.png">

### 5.目录结构

<img src="D:\user\notes\picture\Maven04.png">

`总结：Eclipse创建Maven工程跳过骨架目录结构完整`

## 三、入门程序

### 1.新建Maven工程

> new --- > project --- > Maven Project  --- > 勾选跳过骨架选择

### 2.补全目录结构

> 在src/main/webapp目录下创建WEB-INF目录并添加web.xml配置文件

### 3.导入坐标

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.0.1</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>jsp-api</artifactId>
    <version>2.1</version>
    <scope>provided</scope>
</dependency>
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
    <version>1.2</version>
</dependency>
```



### 4.编写servlet

```java
public class HelloController extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws 			ServletException, IOException {
        response.getWriter().print("hello Maven!");
    }
}
```
## 四、Maven一键构建

> 右击项目 --- > Run as --- > Maven Build... 输入命令 tomcat:run

<img src="D:\user\notes\picture\Maven06.png">

## 五、Maven聚合工程

说明：依赖传递：子工程继承父工程的坐标

### 1.新建一个Mave工程(父工程)

> new --- > project --- > Maven Project 
>
> `注意 :  父工程打包方式必须是pom`

### 2.新建三个Maven模块（子模块）

> new --- > project --- > Maven Module

### 3.父工程pom.xml

#### 3.1mall_parent

```xml
<modelVersion>4.0.0</modelVersion>
  <groupId>com.myself</groupId>
  <artifactId>mall_parent</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>pom</packaging>
  <modules>
  	<module>mall_order</module>
  	<module>mall_user</module>
  	<module>mall_product</module>
  </modules>
</project>
```



### 4.子工程pom.xml

#### 4.1mall_user

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.myself</groupId>
    <artifactId>mall_parent</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>mall_user</artifactId>
  <packaging>war</packaging>
</project>
```

#### 4.2mall_order

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.myself</groupId>
    <artifactId>mall_parent</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>mall_order</artifactId>
</project>
```

#### 4.3mall_product

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <parent>
    <groupId>com.myself</groupId>
    <artifactId>mall_parent</artifactId>
    <version>0.0.1-SNAPSHOT</version>
  </parent>
  <artifactId>mall_product</artifactId>
</project>
```
## 六、Maven仓库配置

### 1.本地仓库配置

```xml
<localRepository>D:\gec\software\IDEA\repository</localRepository>
```

### 2.中央仓库

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>        
</mirror>
```

## 七、解决jar包冲突

```xml
<dependency>
  <groupId>org.apache.struts</groupId>
  <artifactId>struts-core</artifactId>
  <version>1.3.8</version>
</dependency>
<dependency>
  <groupId>org.hibernate</groupId>
  <artifactId>hibernate-core</artifactId>
  <version>5.0.5.Final</version>
  <exclusions>
    <exclusion>
      <groupId>org.javassist</groupId>
      <artifactId>javassist</artifactId>
    </exclusion>
  </exclusions>
</dependency>
```

## 八、版本锁定的两种方式

### 1.第一种：

```xml
<!--版本锁定，并不会导入依赖-->
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.apache.struts</groupId>
      <artifactId>struts-core</artifactId>
      <version>1.3.8</version>
    </dependency>
  </dependencies>
</dependencyManagement>

<dependencies>
      <dependency>
          <groupId>org.apache.struts</groupId>
          <artifactId>struts-core</artifactId>
      </dependency>
</dependencies>
```

### 2.第二种：

```xml
<properties>
    <spring.version>4.3.12.RELEASE</spring.version>
</properties>
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-webmvc</artifactId>
  <version>${spring.version}</version>
</dependency>
```


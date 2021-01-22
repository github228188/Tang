# springMVC

### 一、 创建工程

> file--- > new --- >project --- > java Enterprise

### 二、添加jar包

> 1. 在web目录下创建一个lib目录
>
> 2. 导入以下jar包（D:\gec\jar\springmvc）：
>
>    commons-logging-1.1.1.jar
>    spring-aop-4.2.4.RELEASE.jar
>    spring-beans-4.2.4.RELEASE.jar
>    spring-context-4.2.4.RELEASE.jar
>    spring-core-4.2.4.RELEASE.jar
>    spring-expression-4.2.4.RELEASE.jar
>    spring-web-4.2.4.RELEASE.jar
>    spring-webmvc-4.2.4.RELEASE.jar
>
> 3. 将jar包Add as Library

### 三、新建springMVC.xml配置文件

> 1. 在src目录下，new --- > file --- > springMVC.xml
> 2. 编辑如下配置：



```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--开启创建spring容器需要扫描的包-->
    <context:component-scan base-package="com.myself"/>
    <!--配置视图解析器-->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <!--开启mvc注解支持-->
    <mvc:annotation-driven/>
</beans>
```

### 四、配置web.xml

>在web --- > WEB-INF --- > web.xml 中配置servlet

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springMVC.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

### 五、编写HomeController

```java
package com.myself.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

@Controller
@RequestMapping("/start")
public class HomeController {
    @RequestMapping("/one")
    public ModelAndView testSpringStart(){
        ModelAndView mv = new ModelAndView();
        mv.setViewName("success");
        return mv;
    }
}
```

### 六、编写发请求页面

> 编辑web/index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
  </head>
  <body>
  <h3><a href="start/one">spring_start</a></h3>
  </body>
</html>
```

### 七、编写响应页面

> 编辑web/pages/success.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    SpringMVC start success!!!
</body>
</html>
```

### 八、图解

<img src="D:\user\notes\picture\springMVC.png" style="zoom: 200%;" >


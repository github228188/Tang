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

### 九、文件上传

#### 1.导入坐标

```xml
<!--文件上传需要的2个坐标-->
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.2.2</version>
</dependency>
<dependency>
  <groupId>commons-io</groupId>
  <artifactId>commons-io</artifactId>
  <version>2.4</version>
</dependency>
```

#### 2.编写前端

##### 2.1说明：

> 1. 表单必须以post请求方式提交
> 2. 表单enctype属性必须为multipart/form-data分段式
> 3. 上传文件类型必须为file

##### 2.2代码：

```xml
<form method="post" enctype="multipart/form-data" action="upload">
    <input type="file" name="pictureFile">
    <input type="submit" value="上传">
</form>
```

#### 3.编写后端

```java
@RequestMapping("/upload")
public ModelAndView fileUpload(MultipartFile pictureFile) throws IOException {
    //把跳转的页面和要返回的数据封装在一起
    ModelAndView mv = new ModelAndView();
    //获得文件的名字
    String filename = pictureFile.getOriginalFilename();
    //文件名处理
    String uuid = UUID.randomUUID().toString().replace("-", "");
    int index = filename.lastIndexOf(".");
    String newFileName = uuid + filename.substring(index);
    //文件上传的位置
    pictureFile.transferTo(new File("D:\\gec\\images\\" + newFileName));
    mv.setViewName("success");
    return mv;
}
```

#### 4.配置文件上传解析器

```xml
<!--springmvc.xml配置文件上传解析器-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="maxUploadSize" value="102000"/>
</bean>
```

### 十、springmvc静态资源处理

```xml
<!--处理静态资源
    mapping:映射项目中的路径
    location:浏览器访问路径
-->
<mvc:resources mapping="/css/**" location="/css/"/>
```

### 十一、ajax前后台发送json数据交互

#### 1.1引入jquery

> + 在webapp下新建一个js目录
> + 将jquery-1.8.3.min.js文件放到js目录下
> + 在jsp页面引入jquery-1.8.3.min.js

#### 1.2编写ajax

```html
<button id="btn">发送ajax请求</button>
<script type="text/javascript" src="${pageContext.request.contextPath}/js/jquery-1.8.3.min.js"></script>
<script>
    $(function () {
        $("#btn").click(function(){
            $.ajax({
                url:"${pageContext.request.contextPath}/testJson",
                contentType:"application/json;charset=utf-8",
                data:{name:"张三",detail:"very good!"},
                success:function (data) {
					console.log(data);
                }
            })
        })
    })
</script>
```

#### 3.导入坐标（json）

```xml
<!--json-->
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-core</artifactId>
  <version>2.4.2</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-annotations</artifactId>
  <version>2.4.0</version>
</dependency>
<dependency>
  <groupId>com.fasterxml.jackson.core</groupId>
  <artifactId>jackson-databind</artifactId>
  <version>2.4.2</version>
</dependency>
```

#### 4.编写后端

```java
/**
* 后台接收前台数据并在控制台打印
*/
@RequestMapping("/testJson")
//ajax发送过来的json数据可以自动封装到对象中
public ModelAndView testJson(ProductWithBLOBs product){
    ModelAndView mv = new ModelAndView();
    System.out.println(product);

    Product p = new Product();
    p.setName("李四");
    p.setCreateDate(new Date());

    mv.setViewName("success");
    return mv;
}
    /**
     * 后端响应json数据到前台
     * ResponesBody注解：
     * 就不会默认跳转到请求路径.jsp
     * 会将数据以json的形式发送给前台
     * @return
     */
    @RequestMapping("/testJsonList")
    @ResponseBody
    public List<Product> testJsonList(){
        List<Product> products = new ArrayList<Product>();
        Product product = new Product("张三",new Date());
        products.add(product);
        return products;
    }
```

### 十二、springMVC请求的三种返回值类型

```html
<h3><a href="returnVoid">returnVoid</a></h3><br>
<h3><a href="returnString">returnString</a></h3><br>
<h3><a href="returnModelAndView">returnModelAndView</a></h3><br>
```

#### 1.void类型

```java
/**
 * 返回值类型为void
 * @param req
 * @param resp
 * @throws Exception
 */
@RequestMapping("/returnVoid")
public void returnVoid(HttpServletRequest req, HttpServletResponse resp) throws Exception {
    //1.请求转发:需自己写转发路径
    //req.getRequestDispatcher("/WEB-INF/views/success.jsp").forward(req,resp);
    //2.重定向
    resp.sendRedirect(req.getContextPath() + "/success.jsp");
}
```

#### 2.String类型

```java
/**
 * 返回值为String类型
 * @return
 */
@RequestMapping("/returnString")
public String returnString(Model model){
    //通过Model设置响应数据
    model.addAttribute("product",new Product("椅子",new Date()));
    //1.转发到某个jsp页面
    //return "success";
    //2.转发到某个路径
    //return "forward:/success";
    //3.重定向到某个路径
    return "redirect:/success";
}
/**
 * 模拟/success请求的路径
 */
@RequestMapping("/success")
public String success(){
    return "success";
}
```

#### 3.ModelAndView类型

```java
/**
 * 返回ModelAndView类型
  * @return
 */
@RequestMapping("returnModelAndView")
public ModelAndView returnModelAndView(){
    ModelAndView mv = new ModelAndView();
    mv.setViewName("success");

    Product product = new Product("桌子",new Date());
    mv.addObject("product",product);
    return mv;
}
```

### 十三、springMVC全局异常处理机制

#### 1.编写一个类自定义异常类（做提示信息）

```java
public class MyException extends Exception{

    //异常提示信息
    private String msg;

    public String getMsg() {
        return msg;
    }

    public void setMsg(String msg) {
        this.msg = msg;
    }

    public MyException(String msg) {
        this.msg = msg;
    }

    public MyException() {
    }
```

#### 2.编写异常处理器（实现HandlerExceptionResolver）

```java
@Controller
public class ExceptionController {
    /**
     * 测试springMVC的异常处理器
     * 正常：返回success.jsp
     * 错误跳转到error.jsp
     * @return
     */
    @RequestMapping("testException")
    public String testException() throws Exception {
        //模拟异常
        try {
            int i = 1/0;
        } catch (Exception e) {
            throw new MyException("异常处理器测试成功！");
        }
        return "success";
    }
```

#### 3.配置异常处理器（跳转到提示页面）

```xml
<!-- 配置异常处理器 -->
<bean id="sysExceptionResolver" class="com.myself.utils.MyExceptionResolver"/>
```

### 十四、sprngMVC自定义类型转换器

#### 1.编写一个类实现Converter<String,Date>

```java
public class DateConverterStringToDate implements Converter<String, Date> {
    //进行类型转换的方法
    @Override
    public Date convert(String s) {
        if (s == null){
            throw new RuntimeException("参数不能为空！！！");
        }
        DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
        try {
            Date date = df.parse(s);
            return date;
        } catch (ParseException e) {
            throw new RuntimeException("类型转换错误！！！");
        }
    }
```

#### 2.注册自定义类型转换器

##### 2.1在springmvc.xml配置文件中编写配置

```xml
<!--两个基本的配置-->
<mvc:annotation-driven conversion-service="conversionService"/>
<!--配置自定义类型转换器-->
<bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
    <property name="converters">
        <set>
            <bean class="com.myself.utils.DateConverterStringToDate"/>
        </set>
    </property>
</bean>
```

### 十五、springMVC自定义拦截器

#### 1.代码

##### 1.1编写一个类实现HandlerInterceptor

```java
public class MyInterceptor1 implements HandlerInterceptor {
    /**
     * 如何调用：按拦截器定义顺序调用
     * 何时调用：配置了就会调用
     * 作用：如何拦截器对请求处理后，还有其他拦截器或其他业务要处理，则返回true,如果不需要处理其他请求则返回false
     */

    @Override
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        return false;
    }
    /**
     * 如何调用：按拦截器定义逆序调用
     * 何时调用：当所有preHandle都返回true是调用（必须所有拦截器都返回true才调用）
     * 作用：Controller处理后调用，处理响应给客户端的request请求
     */
    @Override
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {

    }
    /**
     * 如何调用：按拦截器定义逆序调用
     * 何时调用：当preHandle都返回true是调用（哪一个拦截器返回true调用哪一个）
     * 作用：释放资源
     */
    @Override
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {

    }
}
```

##### 1.2 说明：

> 1. **preHandle方法是controller方法执行前拦截的方法**
>    + **可以使用request或者response跳转到指定的页面**
>    + **return true放行，执行下一个拦截器，如果没有拦截器，执行controller中的方法。**
>    + **return false不放行，不会执行controller中的方法。**
>
> 2. **postHandle是controller方法执行后执行的方法，在JSP视图执行前。**
>    + **可以使用request或者response跳转到指定的页面**
>    + **如果指定了跳转的页面，那么controller方法跳转的页面将不会显示。**
>
> 3. **postHandle方法是在JSP执行后执行**
>    + **request或者response不能再跳转页面了**
> 4. 拦截器与过滤器的区别：
>    + **过滤器**是 servlet 规范中的一部分，任何 java web 工程都可以使用。
>    + **拦截器**是 SpringMVC 框架自己的，只有使用了 SpringMVC 框架的工程才能用。
>    + **过滤器**在 url-pattern 中配置了**/\***之后，可以对所有要访问的静态资源拦截。
>    + **拦截器**它是只会拦截访问的控制器方法，如果访问的是 jsp，html,css,image 或者 js 是不会进行拦截的。
>    + 它也是 AOP 思想的具体应用。

#### 2.在springmvc.xml中配置拦截器类

```xml
<!-- 配置拦截器 -->
<mvc:interceptors>
    <mvc:interceptor>
        <!-- 哪些方法进行拦截 -->
        <mvc:mapping path="/user/*"/>
        <!-- 哪些方法不进行拦截 <mvc:exclude-mapping path=""/> -->
        <!-- 注册拦截器对象 -->
        <bean class="com.myself.utils.MyInterceptor1"/>
    </mvc:interceptor>
</mvc:interceptors>
```

#### 3.图解

<img src="D:\user\notes\picture\springMVC_01.png" style="zoom:150%;"   >

### 十六、验证码登录

#### 1.登录界面

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<html>
<head>
    <title>登录</title>
    <script>
        window.onload = function () {
            document.getElementById("img1").onclick = function () {
                this.src = "${pageContext.request.contextPath}/checkCode?random="+Math.random();
            }
        }
    </script>
</head>
<body>
<form action="login" method="post">
    <div>${msg}</div>
    账号：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    验证码：<input type="text" name="code">
    <img src="${pageContext.request.contextPath}/checkCode" width="100px" height="50px" id="img1"><br>
    <input type="submit" value="登录">
</form>
</body>
</html>
```

#### 2.编写获取验证码Controller

```java
/**
 * 获取验证码
 * @param resp
 * @param session
 * @throws IOException
 */
@RequestMapping("/checkCode")
public void checkCode(HttpServletResponse resp, HttpSession session) throws IOException {
    ICaptcha captcha = CaptchaUtil.createLineCaptcha(200, 100);
    // 将验证码中字符存取到session 中
    String code = captcha.getCode();
    // 验证码用于验证
    session.setAttribute("validateCode", code);
    // 写出验证码
    captcha.write(resp.getOutputStream());
}
```

#### 3.登录

```java
/**
 * 登录
 * @param user
 * @param code
 * @param req
 * @param resp
 * @throws Exception
 */
@RequestMapping("/login")
public void login(User user, String code, HttpServletRequest req,HttpServletResponse resp) throws Exception {
    //判断验证码是否正确
    String validateCode = (String) req.getSession().getAttribute("validateCode");
    if (code.equalsIgnoreCase(validateCode)) {
        //登录
        req.getRequestDispatcher("/WEB-INF/views/success.jsp").forward(req,resp);
    } else {
        req.setAttribute("msg","验证码错误！");
        req.getRequestDispatcher("/login.jsp").forward(req,resp);
    }
}
```
# Spring

## 一、spring整合junit

### 1.导入坐标

```xml
<!--单元测试-->
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.12</version>
  <scope>test</scope>
</dependency>
<!--spring test-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>4.3.7.RELEASE</version>
    <scope>test</scope>
</dependency>
```

2.在测试类上添加注解

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
```


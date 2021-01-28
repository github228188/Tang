# Mybatis

## 一、mybatis 逆向工程

### 1. 导入坐标

```xml
<dependency>
  <groupId>org.mybatis.generator</groupId>
  <artifactId>mybatis-generator-core</artifactId>
  <version>1.3.5</version>
</dependency>
```

### 2. 在工程目录下创建一个mbg.xml配置文件

#### 2.1 右击工程名 --- > new --- > file mbg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--去掉生成的注释-->
        <commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--配置数据库连接信息-->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/ssm_crud"
                        userId="root"
                        password="root">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>
        <!-- 指定Javabean存放的位置 -->
        <javaModelGenerator targetPackage="com.myself.domain" targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- 指定dao接口对应的映射文件的位置-->
        <sqlMapGenerator targetPackage="mappers"  targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- 指定dao接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.myself.dao"  targetProject=".\src\main\java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        <!-- 指定每个表的生成策略-->
        <table tableName="tbl_emp" domainObjectName="Employee"/>
        <table tableName="tbl_dept" domainObjectName="Department"/>

    </context>
</generatorConfiguration>
```

### 3. 创建测试类执行

`注意：执行之前需要创建好mbg.xml配置文件中对应的目录结构`

```java
/**
 * mybatis逆向工程
 */
public class MBGTest {
    public static void main(String[] args) throws Exception{
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        File configFile = new File("mbg.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
        myBatisGenerator.generate(null);
    }
}
```

## 二、mybatis分页插件

### 1. 导入坐标

```xml
<!--mybatis分页插件-->
<dependency>
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId>
  <version>5.0.0</version>
</dependency>
```

### 2. 在mybatis-config.xml配置文件中添加配置

```xml
<!--mybatis分页插件注册-->
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
        <!--分页数据合理化配置-->
        <property name="reasonable" value="true"/>
    </plugin>
</plugins>
```

### 3. 使用分页插件

```java
/**
 * mybatis提供的标准分页插件
 * 1.导入pagehelper坐标
 * 2.在mybatis-config.xml配置文件中注册
 * 3.开始分页
 */
@RequestMapping("/emps")
//@ResponseBody:将响应的数据以json形式返回
@ResponseBody
public Msg findEmpsWithJson(@RequestParam(value = "pn", defaultValue = "1") Integer pn) {
    //每页多少个数据，当前页是多少
    PageHelper.startPage(pn, 5);
    //startPage后面紧跟的查询是分页查询
    List<Employee> emps = employeeService.findEmps();
    //使用pageInfo 封装查询后的结果,navigate中的页码有五个
    PageInfo<Employee> pageInfo = new PageInfo<>(emps, 5);
    return Msg.success().add("pageInfo", pageInfo);
}
```

## 三、mybatis-spring批处理

### 1. 导入坐标

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis-spring</artifactId>
  <version>1.3.1</version>
</dependency>
```

### 2.在application.xml配置文件中添加配置

```xml
<!--sqlSessionFactory:读取mybatis-config.xml配置文件内容-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <!--数据源-->
    <property name="dataSource" ref="dataSource"/>
    <!--映射文件的位置-->
    <property name="mapperLocations" value="classpath:mappers/*Mapper.xml"/>
    <!--mybatis全局配置文件的位置-->
    <property name="configLocation" value="classpath:mybatis-config.xml"/>
</bean>

<!--spring-mybatis批处理-->
<bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
    <constructor-arg ref="sqlSessionFactory"/>
    <constructor-arg name="executorType" value="BATCH"/>
</bean>
```

### 3.使用批处理

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class MapperTest {

    @Autowired
    EmployeeMapper employeeMapper;
    //spring-mybatis的批处理session（配置，使用）
    @Autowired
    SqlSession sqlSession;

    /**
     * 测试添加员工
     * 配置一个批处理添加
     */
    @Test
    public void testEmpAdd(){
        EmployeeMapper mapper = sqlSession.getMapper(EmployeeMapper.class);
        for (int i = 0; i < 1000; i++) {
            String uid = UUID.randomUUID().toString().substring(0, 5) + i;
            mapper.insertSelective(new Employee(null,uid,"M",uid + "@myself.com",3));
        }
    }
}
```
# Activiti

## 一、Eclipse集成Activiti插件安装

<img src="D:\user\notes\picture\activiti02.png">

<img src="D:\user\notes\picture\activiti02.png">

<img src="D:\user\notes\picture\activiti03.png">

<img src="D:\user\notes\picture\activiti04.png">

<img src="D:\user\notes\picture\activiti05.png">

## 二、activiti入门案例

### 1.准备

> + 创建一个空的数据库activiti_start

### 2.新建一个Java工程

> + new --- > Project --- > Java Project 

### 3.导入jar包

> + 在工程目录下新建一个lib文件夹
> + 导入如下jar包

```lisp
activation-1.1.jar
activiti-bpmn-converter-5.13.jar
activiti-bpmn-layout-5.13.jar
activiti-bpmn-model-5.13.jar
activiti-common-rest-5.13.jar
activiti-engine-5.13.jar
activiti-json-converter-5.13.jar
activiti-rest-5.13.jar
activiti-simple-workflow-5.13.jar
activiti-spring-5.13.jar
aopalliance-1.0.jar
commons-dbcp-1.4.jar
commons-email-1.2.jar
commons-fileupload-1.2.2.jar
commons-io-2.0.1.jar
commons-lang-2.4.jar
commons-pool-1.5.4.jar
h2-1.3.170.jar
hamcrest-core-1.3.jar
jackson-core-asl-1.9.9.jar
jackson-mapper-asl-1.9.9.jar
javaGeom-0.11.0.jar
jcl-over-slf4j-1.7.2.jar
jgraphx-1.10.4.2.jar
joda-time-2.1.jar
junit-4.11.jar
log4j-1.2.17.jar
mail-1.4.1.jar
mybatis-3.2.2.jar
mysql-connector-java-5.1.41.jar
org.restlet-2.0.15.jar
org.restlet.ext.fileupload-2.0.15.jar
org.restlet.ext.jackson-2.0.15.jar
org.restlet.ext.servlet-2.0.15.jar
slf4j-api-1.7.2.jar
slf4j-log4j12-1.7.2.jar
spring-aop-3.1.2.RELEASE.jar
spring-asm-3.1.2.RELEASE.jar
spring-beans-3.1.2.RELEASE.jar
spring-context-3.1.2.RELEASE.jar
spring-core-3.1.2.RELEASE.jar
spring-expression-3.1.2.RELEASE.jar
spring-jdbc-3.1.2.RELEASE.jar
spring-orm-3.1.2.RELEASE.jar
spring-tx-3.1.2.RELEASE.jar
```



### 4.编写入门代码

```java
public class ActivitiStart {
	public static void main(String[] args) {
		//创建一个单例的流程引擎配置对象
		ProcessEngineConfiguration configuration = ProcessEngineConfiguration.createStandaloneProcessEngineConfiguration();
		//配置连接数据库的信息
		configuration.setJdbcDriver("com.mysql.jdbc.Driver");
		configuration.setJdbcUrl("jdbc:mysql://localhost:3306/activiti_start");
		configuration.setJdbcUsername("root");
		configuration.setJdbcPassword("root");
		//设置建表策略
		/**
		 * false : 没有表会报错
		 * true  : 有表修改表，没有表就创建表
		 * create-drop : 创建表后删除表
		 */
		configuration.setDatabaseSchemaUpdate("true");
		//通过流程引擎配置对象创建流程引擎对象
		ProcessEngine engine = configuration.buildProcessEngine();
		System.out.println("engine: " + engine);
		
	}
}
```

## 三、activiti-5.13.jar目录结构

<img src="D:\user\notes\picture\activiti01.png">


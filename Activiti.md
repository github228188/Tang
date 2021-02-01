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

#### 1.新建一个Java工程

> + new --- > Project --- > Java Project 

#### 2.导入jar包

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



### 2.第一种方式：

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

### 3.第二种方式：

#### 1.新建一个activiti.cfg.xml配置文件

#### 2.添加如下配置

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-2.5.xsd
http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.0.xsd">
	<bean id="processEngineConfiguration" class="org.activiti.engine.impl.cfg.StandaloneProcessEngineConfiguration">
		<property name="jdbcDriver" value="com.mysql.jdbc.Driver"></property>
		<property name="jdbcUrl" value="jdbc:mysql://localhost:3306/activiti_start?useSSL=false"></property>
		<property name="jdbcUsername" value="root"></property>
		<property name="jdbcPassword" value="root"></property>
		<property name="databaseSchemaUpdate" value="true"></property>
	</bean>
</beans>
```

#### 3.编写测试类

```java
@Test
public void fun2() {
    ProcessEngineConfiguration processEngineConfiguration = 
        ProcessEngineConfiguration.createProcessEngineConfigurationFromResource("activiti.cfg.xml");
    ProcessEngine processEngine = processEngineConfiguration.buildProcessEngine();
    System.out.println(processEngine);

}
```

### 4.第三种方式：

#### 1.编写测试类

```java
@Test
public void fun3() {
	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();
	System.out.println(processEngine);
}
```
#### 2.底层实现

<img src="D:\user\notes\picture\activiti06.png"  >

## 三、activiti-5.13.jar目录结构

<img src="D:\user\notes\picture\activiti01.png">

## 四、工作流执行过程

### 1.准备

##### 1.新建一个资源包

<img src="D:\user\notes\picture\activiti07.png">

##### 2.在resources目录下建一个diagram包

##### 3.在digram包下new Activiti Diagram

###### 1.节点

<img src="D:\user\notes\picture\activiti08.png">

###### 2.代办人

<img src="D:\user\notes\picture\activiti09.png">

###### 3.流程id

<img src="D:\user\notes\picture\activiti10.png">

##### 4.编写测试类

```java
public class HelloWorld {
	ProcessEngine processEngine= ProcessEngines.getDefaultProcessEngine();
	/**
	 * 流程部署
	 */
	@Test
	public void DeployProcess() {
		Deployment deploy = processEngine
		.getRepositoryService()
		.createDeployment()
		.name("第一个部署流程测试")
		.addClasspathResource("digram/holiday.bpmn")
		.addClasspathResource("digram/holiday.png")
		.deploy();
		
		System.out.println("部署ID:" + deploy.getId());
		System.out.println("部署name:" + deploy.getName());
        //操作了act_re_procdef流程定义、act_re_deployment流程部署、act_ge_bytearray资源表
	}
	
	
	/**
	 * 流程启动
	 */
	@Test
	public void StartProcess() {
		//根据流程的id启动流程 得到流程的processInstance
		String processDefinitionKey = "holiday";
		ProcessInstance processInstance = processEngine.getRuntimeService()
		.startProcessInstanceByKey(processDefinitionKey);
		
		System.out.println("流程定义ID:" + processInstance.getProcessDefinitionId());
		System.out.println("流程实例ID:" + processInstance.getProcessInstanceId());
        
        //操作了act_ru_execution、act_hi_procinst、act_ru_variable
	}
	
	
	/**
	 * 查看当前代办人任务列表
	 */
	@Test
	public void taskList() {
		List<Task> list = processEngine.getTaskService()
		.createTaskQuery()
		.taskAssignee("李四")
		.list();
		
		if(list != null && list.size() > 0) {
			for (Task task : list) {
				System.out.println("任务id：" + task.getId());
				System.out.println("任务名称：" + task.getName());
				System.out.println("创建时间：" + task.getCreateTime());
				System.out.println("任务代办人：" + task.getAssignee());
				System.out.println("流程定义id：" + task.getProcessDefinitionId());
				System.out.println("流程实例id：" + task.getProcessInstanceId());
			}
		}
	}
	
	/**
	 * 完成任务：跳到下一个节点
	 */
	@Test
	public void completeTask() {
		String taskId = "202";
		processEngine.getTaskService()
		.complete(taskId );
		
		System.out.println("任务完成！！");
	}
}

```

## 五、部署zip流程

```java
public class DeployZip {
	//获取流程引擎对象
	ProcessEngine processEngine= ProcessEngines.getDefaultProcessEngine();
	@Test
	public void deployProcess() {
		InputStream in = DeployZip.class.getClassLoader().getResourceAsStream("digram/holiday.zip");;
		ZipInputStream zipInputStream = new ZipInputStream(in );
		Deployment deploy = processEngine.getRepositoryService()
		.createDeployment()
		.name("部署zip文件测试")
		.addZipInputStream(zipInputStream )
		.deploy();

		System.out.println("部署ID:" + deploy.getId());
		System.out.println("部署name:" + deploy.getName());
	}
}
```



## 六、查询流程定义

```java
/**
 * 流程定义查询
 * @author Tang
 *
 */
public class ProcessDefinationQuery {
	//获取流程引擎对象	
	ProcessEngine processEngine= ProcessEngines.getDefaultProcessEngine();
	@Test
	public void processDefinationQuery() {
		String processDefinitionKey = "holiday";
		List<ProcessDefinition> list = processEngine.getRepositoryService()
		.createProcessDefinitionQuery()
		.processDefinitionKey(processDefinitionKey)
		.orderByDeploymentId()
		.asc()
		.list();
		
		if(list != null && list.size() > 0) {
			for (ProcessDefinition processDefinition : list) {
				System.out.println("key:" + processDefinition.getKey());
				System.out.println("version:" + processDefinition.getVersion());
				System.out.println("name:" + processDefinition.getName());
				System.out.println("id:" + processDefinition.getId());
				System.out.println("DeployId:" + processDefinition.getDeploymentId());
			}
		}
	}
}
```

## 七、删除流程定义

```java
/**
 * 删除流程定义
 * @author Tang
 *
 */
public class ProcessDefinationDelete {
	
	//获取流程引擎对象	
	ProcessEngine processEngine= ProcessEngines.getDefaultProcessEngine();
	@Test
	public void deleteProcessDefination() {
		String deploymentId = "401";
		processEngine.getRepositoryService()
		.deleteDeployment(deploymentId,true);
		
		System.out.println("删除已完成！");
	}
}
```

## 八、获取并保存流程图

```java
/**
 * 获取并保存流程图
 * @author Tang
 *
 */
public class GetAndSaveProcessPicture {

	//获取流程引擎对象	
	ProcessEngine processEngine= ProcessEngines.getDefaultProcessEngine();
		
	@Test
	public void getAndSaveProcessPucture() throws Exception {
		//部署Id
		String deploymentId = "1";
		
		//图片名称
		String resourceName = "";
		
		List<String> list = processEngine.getRepositoryService()
		.getDeploymentResourceNames(deploymentId);
		
		if(list != null && list.size() > 0) {
			for (String s : list) {
				if(s.indexOf(".png")> -1) {
					resourceName = s;
				}
			}
			
		}
		//activiti提供跟据部署id和资源名获取到文件流
		InputStream is = processEngine.getRepositoryService().getResourceAsStream(deploymentId, resourceName);

		//System.out.println(resourceName);
		FileUtils.copyInputStreamToFile(is, new File("D:\\" + resourceName));
		System.out.println("圖片下載完成");
		
	}
}

```

## 九、查询任务的历史记录

```java
/**
 * 任务历史记录查询
 * @author Tang
 *
 */
public class TaskHistoryQuery {

	//获取流程引擎对象	
	ProcessEngine processEngine= ProcessEngines.getDefaultProcessEngine();
	
	@Test
	public void taskHistoryQuery() {
		List<HistoricTaskInstance> list = processEngine.getHistoryService()
		.createHistoricTaskInstanceQuery()
		.taskAssignee("张三")
		.list();
		
		if(list != null && list.size() > 0) {
			for (HistoricTaskInstance historicTaskInstance : list) {
				System.out.println("历史任务名称 ：" + historicTaskInstance.getName());
				System.out.println("历史任务代办人 ：" + historicTaskInstance.getAssignee());
				System.out.println("历史任务开始时间 ：" + historicTaskInstance.getStartTime());
				System.out.println("历史任务结束时间 ：" + historicTaskInstance.getEndTime());
			}
		}
	}
}
```



## 十、流程变量

```java
/**
 * 流程变量的使用
 * @author Tang
 *
 */
public class ProcessValiable {
	

	//获取流程引擎对象	
	ProcessEngine processEngine= ProcessEngines.getDefaultProcessEngine();
	/**
	 * 设置流程变量
	 */
	@Test
	public void setValiable() {
		String taskId = "802";
		String variableName = "money";
		Object value = 1000;
		processEngine.getTaskService()
		.setVariable(taskId, variableName, value);
	}
	/**
	 * 完成流程
	 */
	@Test
	public void completeTask() {
		processEngine.getTaskService().complete("802");
		System.out.println("任务完成！！");
	} 
	/**
	 * 获得流程变量的值
	 */
	@Test
	public void getVailable() {
		String taskId = "1002";
		String variableName = "money";
		Object variable = processEngine.getTaskService().getVariable(taskId, variableName);
		System.out.println(variable);
	}
}
```

## 十一、流程连线

### 1.通过流程连线来控制流程走向

#### 1.制定流程图

<img src="D:\user\notes\picture\activiti11.png">

##### 1.细节处理

<img src="D:\user\notes\picture\activiti12.png">

#### 2.编写测代码

```java
/**
 * 通过流程连线来控制流程走向
 * 
 * @author Tang
 *
 */
public class ProcessFlows {

	// 获取流程引擎对象
	ProcessEngine processEngine = ProcessEngines.getDefaultProcessEngine();

	/**
	 * 部署流程
	 */
	@Test
	public void DeployProcess() {
		Deployment deploy = processEngine.getRepositoryService().createDeployment().name("部署ProcessFlows流程")
				.addClasspathResource("digram/ProcessFlows.bpmn").addClasspathResource("digram/ProcessFlows.png").deploy();
		System.out.println(deploy.getName() + "部署成功！");
		System.out.println("部署id为：" + deploy.getId());

	}

	/**
	 * 启动流程
	 */
	@Test
	public void StartProcess() {
		String processDefinitionKey = "ProcessFlows";

		ProcessInstance processInstance = processEngine.getRuntimeService()
				.startProcessInstanceByKey(processDefinitionKey);

		System.out.println("流程实例id:" + processInstance.getProcessInstanceId());
		System.out.println("流程定义id:" + processInstance.getProcessDefinitionId());
	}

	/**
	 * 流程连线控制流程走向
	 */
	@Test
	public void processFlows() {
		String taskId = "1204";

		Map<String, Object> variables = new HashMap<String, Object>();
		variables.put("message", "同意");
		processEngine.getTaskService().complete(taskId, variables);
		System.out.println("任务已完成！！");
	}
}
```


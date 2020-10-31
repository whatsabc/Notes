# Spring

---



```java
/**
*账户业务层的接口
*/
public interface IAccountService {
	/*
	*模拟保存账户
	*/
	void saveAccount();
}
```



```java
public interface AccountServiceImpl implements IAccountService{
	//private IAcountDao accountDao=new AccountDaoImpl();
	private IAcountDao accountDao=BeanFactory.getBean("account");
	public void saveAccount(){
		accountDao.saveAccount();
	}
}
```



```java
//账户的持久层接口
public interface IAccountDao{
	void saveAccont();
}
```



```java
//账户的持久层实现类
public interface AccountDaoImpl implements IAccountDao {
	public void saveAccont(){
		System.out.println("保存了账户")
	}
}
```



```java
//模拟表现层，用于调用业务
public class Client{
	public static void main(String args[]){
		//IAccountService as=new AccountServiceImpl();
		IAccountService as=(IAccountService)BeanFactory.getBean("accontService");
		as.saveAccount();
	}
}
```

如何降低上面各模块之间的耦合性？

下面准备我们的配置文件，bean.properties

```
accountService=com.xxx.service.impl.AccountServiceImpl
accountDao=com.xxx.service.impl.AccountDaoImpl
```



```java
/*
*一个创建Bean对象的工厂
*Bean:在计算机英语中，有可重用组建的含义
*JavaBean:用Java语言编写的可重用组件
*javabean的定义范围要大于实体类
*创建我们的service和dao对象的

*需要一个配置文件来配置我们的service和dao（唯一标志=全限定类名，key=value）
*通过读取配置文件中配置的内容，反射创建对象

*配置文件可以是xml也可以是properties
*/
public class BeanFactory{
	//定义一个properties对象
	private static Properties props;
    
	//定义一个Map，用于创建我们要存放的对象，我们把它称之为容器
	private static Map<String,Object> beans;
    
	//使用静态代码块为Properties对象赋值
	static {
		try{
			props=new Properties();
			//获取properties文件的流对象
			InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
			props.load(in);
			//实例化容器
			beans=new HashMap<String,Object>();
			//取出配置文件中所有的KEY
			Enumeration<String> keys=props.keys();
			//遍历枚举
			while(keys.hasMoreElements()){
				//取出每个key
				String key=keys.nextElement().toString();
				//根据key获取value
				String beanPath=props.setProperty(key);
				//反射创建对象
				Object value=Class.forName(beanPath).newInstance();
				//把key和value存入容器中
				bean.put(key,value);
			}
		} catch(Exception e){
			throw new ExceptionInInitializerError("初始化properties失败！");
		}

	}
	
	//由于我们把每个多例对象在上面已经存入了Map中去，所以下面的方法也就不需要了
    
	/*
	//根据bean的名称获取bean对象
	public Object getBean(String beanName){
		Object bean=null;
		try{
			String beanPath=props.getProperty(beanName);
		bean=Class.forName(beanPath).newInstance();//每次都会调用默认构造函数创建对象，当创建多个对象后，如果不及时进行保存，有可能会被JVM垃圾回收
		}catch(Exception e){
			e.printStackTrace();
		}
		return bean;
	}
	*/
	
	public Object getBean(String beanName){
		return beans.get(beanName);
	}
}
```

## IOC 控制反转

削减计算机程序的耦合(解除我们代码中的依赖关系)。

## 使用Spring的IOC解决程序耦合

### 基于XML的配置

#### 在类的根路径下创建一个任意名称的 xml 文件

```xml
给配置文件导入约束：
/spring-framework-5.0.2.RELEASE/docs/spring-framework-reference/html5/core.html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

#### 第三步：让 spring 管理资源，在配置文件中配置 service 和 dao

```xml
<!-- bean 标签：用于配置让 spring 创建对象，并且存入 ioc 容器之中
 id 属性：对象的唯一标识。
 class 属性：指定要创建对象的全限定类名
-->
<!-- 配置 service -->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
</bean>
<!-- 配置 dao -->
<bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"></bean>
```

#### 测试配置是否成功

```java
/**
* 模拟一个表现层
* @author 黑马程序员
* @Company http://www.ithiema.com
* @Version 1.0
*/
public class Client {
	/**
	 * 使用 main 方法获取容器测试执行
	*/
	public static void main(String[] args) {
		//1.使用 ApplicationContext 接口，就是在获取 spring 容器
		ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
		//2.根据 bean 的 id 获取对象
		IAccountService aService = (IAccountService) ac.getBean("accountService");
		IAccountDao aDao = ac.getBean("accountDao",IAccountDao.class);

		System.out.println(aService);
		System.out.println(aDao);
	}
}
```

运行结果：

```
com.itheima.serviceimpl.AccountServiceImpl@5609159b
com.itheima.dao.lmp.AccountDaoImp@2118cddf
```

#### BeanFactory 和 ApplicationContext 的区别

```
BeanFactory 才是 Spring 容器中的顶层接口。
ApplicationContext 是它的子接口。
BeanFactory 和 ApplicationContext 的区别： 
	创建对象的时间点不一样。 
		ApplicationContext：只要一读取配置文件，默认情况下就会创建对象。
		BeanFactory：什么使用什么时候创建对象。
```

#### ApplicationContext 接口的实现类

```
ClassPathXmlApplicationContext： 它是从类的根路径下加载配置文件 推荐使用这种FileSystemXmlApplicationContext： 它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。 AnnotationConfigApplicationContext: 当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。
```

### IOC 中 bean 标签和管理对象细节

#### 实例化bean的三种方式

```
第一种方式：使用默认无参构造函数
<!--在默认情况下：
它会根据默认无参构造函数来创建类对象。如果 bean 中没有默认无参构造函数，将会创建失败。
-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"/>
```

```
第二种方式：spring 管理静态工厂-使用静态工厂的方法创建对象
/**
* 模拟一个静态工厂，创建业务层实现类
*/
public class StaticFactory {
	public static IAccountService createAccountService(){
		return new AccountServiceImpl();
	}
}
<!-- 此种方式是:
使用 StaticFactory 类中的静态方法 createAccountService 创建对象，并存入 spring 容器
id 属性：指定 bean 的 id，用于从容器中获取
class 属性：指定静态工厂的全限定类名
factory-method 属性：指定生产对象的静态方法
-->
<bean id="accountService"
 class="com.itheima.factory.StaticFactory"
 factory-method="createAccountService"></bean>
```

```
第三种方式：spring 管理实例工厂-使用实例工厂的方法创建对象
/**
* 模拟一个实例工厂，创建业务层实现类
* 此工厂创建对象，必须现有工厂实例对象，再调用方法
*/
public class InstanceFactory {
	public IAccountService createAccountService(){
		return new AccountServiceImpl();
	}
}
<!-- 此种方式是：
先把工厂的创建交给 spring 来管理。
然后在使用工厂的 bean 来调用里面的方法
factory-bean 属性：用于指定实例工厂 bean 的 id。
factory-method 属性：用于指定实例工厂中创建对象的方法。
-->
<bean id="instancFactory" class="com.itheima.factory.InstanceFactory"></bean>
<bean id="accountService"
 factory-bean="instancFactory"
 factory-method="createAccountService"></bean>
```

#### bean 标签

```
作用：
	用于配置对象让 spring 来创建的。
	默认情况下它调用的是类中的无参构造函数。如果没有无参构造函数则不能创建成功。
属性：
	id：给对象在容器中提供一个唯一标识。用于获取对象。
	class：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。
	scope：指定对象的作用范围。
		* singleton: 默认值，单例的.
		* prototype: 多例的.
		* request: WEB项目中,Spring创建一个Bean的对象,将对象存入到request域中.
		* session: WEB项目中,Spring创建一个Bean的对象,将对象存入到session域中.
		* global-session: WEB项目中,应用在Portlet环境.如果没有Portlet环境那么globalSession相当于 session.
	init-method：指定类中的初始化方法名称。
	destroy-method：指定类中销毁方法名称。
```

#### bean对象的作用范围和生命周期

```
单例对象：scope="singleton"
	一个应用只有一个对象的实例。它的作用范围就是整个引用。
	生命周期：
		对象出生：当应用加载，创建容器时，对象就被创建了。
		对象活着：只要容器在，对象一直活着。
		对象死亡：当应用卸载，销毁容器时，对象就被销毁了。
多例对象：scope="prototype"
	每次访问对象时，都会重新创建对象实例。
	生命周期：
		对象出生：当使用对象时，创建新的对象实例。
		对象活着：只要对象在使用中，就一直活着。
		对象死亡：当对象长时间不用时，被 java 的垃圾回收器回收了。
```

### spring的依赖注入

#### 概念

- 依赖注入：Dependency Injection。它是 spring 框架核心 ioc 的具体实现。我们的程序在编写时，通过控制反转，把对象的创建交给了 spring，但是代码中不可能出现没有依赖的情况。
- IOC的作用：IOC解耦只是降低他们的依赖关系，但不会消除。例如：我们的业务层仍会调用持久层的方法。
- 依赖关系的管理：那这种业务层和持久层的依赖关系，在使用 spring 之后，就让 spring 来维护了。简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。
- 依赖关系的维护：就称之为依赖注入

- 依赖注入：
  - 能注入的数据：有三类
    - 基本类型和String类型
    - 其他bean类型（在配置文件中或者注解配置过的bean）
    - 复杂类型/集合类型
  - 注入的方式：三种
    - 使用构造函数提供
    - 使用set方法提供
    - 使用注解提供

#### 构造函数注入

顾名思义，就是使用类中的构造函数，给成员变量赋值。注意，赋值的操作不是我们自己做的，而是通过配置的方式，让 spring 框架来为我们注入。具体代码如下：

```java
/**
*/
public class AccountServiceImpl implements IAccountService {
	private String name;
	private Integer age;
	private Date birthday;
	public AccountServiceImpl(String name, Integer age, Date birthday) {
		this.name = name;
		this.age = age;
		this.birthday = birthday;
	}
	@Override
	public void saveAccount() {
		System.out.println(name+","+age+","+birthday);
	}
}
```


```xml
<!-- 使用构造函数的方式，给 service 中的属性传值
要求：
	类中需要提供一个对应参数列表的构造函数。
涉及的标签：
	constructor-arg
属性：
	index:指定参数在构造函数参数列表的索引位置
	type:指定参数在构造函数中的数据类型
	name:指定参数在构造函数中的名称 用这个找给谁赋值
=======上面三个都是找给谁赋值，下面两个指的是赋什么值的==============
	value:它能赋的值是基本数据类型和 String 类型
	ref:它能赋的值是其他 bean 类型，也就是说，必须得是在配置文件中配置过的 bean
-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
<constructor-arg name="name" value="张三"></constructor-arg>
<constructor-arg name="age" value="18"></constructor-arg>
<constructor-arg name="birthday" ref="now"></constructor-arg>
</bean>
<bean id="now" class="java.util.Date"></bean>
```

优势：在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。

弊端：改变了bean对象的实例化方法，使我们在创建对象时，如果用不到这些数据，也必须提供。

#### set 方法注入

```java
顾名思义，就是在类中提供需要注入成员的 set 方法。具体代码如下：
/** */
public class AccountServiceImpl implements IAccountService {
	private String name;
	private Integer age;
	private Date birthday;
	public void setName(String name) {
		this.name = name;
	}
	public void setAge(Integer age) {
		this.age = age;
	}
	public void setBirthday(Date birthday) {
		this.birthday = birthday;
	}
	@Override
	public void saveAccount() {
		System.out.println(name+","+age+","+birthday);
	}
}
```

```xml
<!-- 通过配置文件给 bean 中的属性传值：使用 set 方法的方式
涉及的标签：
	property
属性：
	name：找的是类中 set 方法后面的部分
	ref：给属性赋值是其他 bean 类型的
	value：给属性赋值是基本数据类型和 string 类型的
实际开发中，此种方式用的较多。
-->
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
<property name="name" value="test"></property>
<property name="age" value="21"></property>
<property name="birthday" ref="now"></property>
</bean>
<bean id="now" class="java.util.Date"></bean>
```

## IOC注解

### 常用的IOC注解和分类

#### @Component

#### @Controller @Service @Repository

```xml
曾经XML的配置
<bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl" scope="" init-method="" destroy-method="">
    <property name="" value=""|ref=""></property>
</bean>
用于创建对象的
	他们的作用就和在XML配置文件中编写一个bean标签实现的功能是一样的
	@Component:
			作用：用于把当前类对象存入spring容器中
			属性：
				value:用于执行bean的id，默认为当前类名，首字母改小写
	
	下面三个注解都是针对一个的衍生注解，他们的作用及属性都是一模一样的。他们只不过是提供了更加明确的语义化。
	@Controller：一般用于表现层的注解。
	@Service：一般用于业务层的注解。
	@Repository：一般用于持久层的注解。
	细节：如果注解中有且只有一个属性要赋值时，且名称是 value，value 在赋值是可以不写。

用于注入数据的
	他们的作用就和在XML配置文件中的bean标签中写一个<property>标签的作用是一样的
用于改变作用范围的
	就在bean标签中使用scope属性实现的功能是一样的
和生命周期相关的
	他们的作用就和在bean标签中使用init-method和destroy-method的作用是一样的
```

## AOP

### AOP概述

#### 什么是AOP

简单的说它就是把我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的 基础上，对我们的已有方法进行增强。

### 动态代理

#### 动态代理的特点

```
字节码随用随创建，随用随加载。
它与静态代理的区别也在于此。因为静态代理是字节码一上来就创建好，并完成加载。
装饰者模式就是静态代理的一种体现。
```

#### 两种方式

```
基于接口的动态代理
	提供者：JDK 官方的 Proxy 类。
	要求：被代理类最少实现一个接口。
基于子类的动态代理
	提供者：第三方的 CGLib，如果报 asmxxxx 异常，需要导入 asm.jar。
	要求：被代理类不能用 final 修饰的类（最终类）。
```

#### 使用 JDK 官方的 Proxy 类创建代理对象

准备一个生产者

```java
public class Produce implement IProduce{
	public void saleProduct(float money){
		System.out.println("销售产品，拿到钱："+money);
	}
	
	public void afterService(float money){
		System.out.println("提供售后服务，并拿到钱："+money);
	}
}
```



```java
public interface IProduce{
	public void saleProduct(float money);
	public void afterService(float money);
}
```



```java
public clsaa Client{
	public static void main(String[] args){
		final Produce producer=new Produce()
		/**
		* 动态代理：
		* 	特点：字节码随用随创建，随用随加载
		* 	作用：不久改源码的基础上对方法增强
		* 	分类：
		* 		基于接口的动态代理
		* 		基于子类的动态代理
		* 	基于接口的动态代理：
		* 		涉及到的类：Proxy
		* 		提供者：JDK官方
		* 创建的方式
		* 	Proxy.newProxyInstance(三个参数)
		* 创建代理对象的要求：
		* 	被代理类最少实现一个接口，如果没有则不能使用
		* newProxyInstance方法的参数含义：
		* 	ClassLoader：用于加载代理对象字节码的，和被代理对象使用相同的类加载器。
		* 	Class[]：字节码数组，和被代理对象具有相同的行为。实现相同的接口。（代理谁就写它的接口）
		* 	InvocationHandler：用于提供增强的代码，让我们写如何代理。（通常都是写一个该接口的实现类，通常情况下都是匿名内部类，但不是必须的）此接口的实现类都是谁用谁写
		*/
		IProduce proxyProducer=(IProduce)Proxy.newProxyInstance(produce.getClass.getClassLoader(),produce.getClass.getInterfaces(),new InvocationHandler(){
			/**
			* 执行被代理对象的任何方法，都会经过该方法。
			* 此方法有拦截的功能。
			*
			* 参数的含义：
			* proxy：代理对象的引用。不一定每次都用得到
			* method：当前执行的方法对象
			* args：执行方法所需的参数
			* 返回值：
			* 当前执行方法的返回值（和被代理对象方法有相同的返回值）
			*/

			@Override
			public Object invoke(Object proxy,Method,Object[] args) throws Throwable{
				//提供增强的代码
				return method.invoke(producer,args);
			}
		});
	}
}
```


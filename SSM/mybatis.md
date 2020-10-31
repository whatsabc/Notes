# mybatis框架

---

## 1、什么是框架？

它是我们软件开发中的一套解决方案，不同的框架解决的是不同的问题。

使用框架的好处：框架封装了很多的细节，使开发者可以使用极简的方式实现功能。大大提高开发效率。

## 2、三层架构

表现层：是用于展示数据的

业务层：是处理业务需求
	
持久层：是和数据库交互的

## 3、持久层技术解决方案

### JDBC技术：

Connection

PreparedStatement

ResultSet

### Spring的JdbcTemplate：

Spring中对jdbc的简单封装

### Apache的DBUtils：

它和Spring的JdbcTemplate很像，也是对Jdbc的简单封装

**以上这些都不是框架**

JDBC是规范，Spring的JdbcTemplate和Apache的DBUtils都只是工具类

## 4、mybatis的概述

mybatis是一个持久层框架，用java编写的。

它封装了jdbc操作的很多细节，使开发者只需要关注sql语句本身，而无需关注注册驱动，创建连接等繁杂过程

它使用了ORM思想实现了结果集的封装。

### ORM思想

Object Relational Mappging对象关系映射

简单的说：就是把数据库表和实体类及实体类的属性对应起来，让我们可以操作实体类就实现操作数据库表。

```
user			User
id			userId
user_name		userName
```

今天我们需要做到，实体类中的属性和数据库表的字段名称保持一致。

```
user			User
id			id
user_name		user_name
```
## 5、mybatis的入门

### mybatis的环境搭建

第一步：创建maven工程并导入坐标

第二步：创建实体类和dao的接口

第三步：创建Mybatis的主配置文件SqlMapConifg.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
 PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 配置 mybatis 的环境 -->
	<environments default="mysql">
		<!-- 配置 mysql 的环境 -->
		<environment id="mysql">
		<!-- 配置事务的类型 -->
		<transactionManager type="JDBC"></transactionManager>
		<!-- 配置连接数据库的信息：用的是数据源(连接池) -->
		<dataSource type="POOLED">
			<property name="driver" value="com.mysql.jdbc.Driver"/>
			<property name="url" value="jdbc:mysql://localhost:3306/ee50"/>
			<property name="username" value="root"/>
			<property name="password" value="1234"/>
		</dataSource>
	</environment>
</environments>


	<!-- 告知 mybatis 映射配置的位置 -->
	<mappers>
		<mapper resource="com/itheima/dao/IUserDao.xml"/>
	</mappers>
</configuration>
```

第四步：创建映射配置文件IUserDao.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
 PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
 "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.dao.IUserDao">
	<!-- 配置查询所有操作 -->
	<select id="findAll" resultType="com.itheima.domain.User">
		select * from user
	</select>
</mapper>
```

### 环境搭建的注意事项：

- 第一个：创建IUserDao.xml 和 IUserDao.java时名称是为了和我们之前的知识保持一致。在Mybatis中它把持久层的操作接口名称和映射文件也叫做：Mapper所以：IUserDao 和 IUserMapper是一样的
- 第二个：在idea中创建目录的时候，它和包是不一样的。包在创建时：com.itheima.dao它是三级结构目录在创建时：com.itheima.dao是一级目录
- 第三个：mybatis的映射配置文件位置必须和dao接口的包结构相同
- 第四个：映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名
- 第五个：映射配置文件的操作配置（select），id属性的取值必须是dao接口的方法名

当我们遵从了第三，四，五点之后，我们在开发中就无须再写dao的实现类。

### mybatis的入门案例

第一步：读取配置文件

第二步：创建SqlSessionFactory工厂

第三步：创建SqlSession

第四步：创建Dao接口的代理对象

第五步：执行dao中的方法

第六步：释放资源

```java
//1.读取配置文件
InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
//2.创建 SqlSessionFactory 的构建者对象
SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
//3.使用构建者创建工厂对象 SqlSessionFactory
SqlSessionFactory factory = builder.build(in);
//4.使用 SqlSessionFactory 生产 SqlSession 对象
SqlSession session = factory.openSession();
//5.使用 SqlSession 创建 dao 接口的代理对象
IUserDao userDao = session.getMapper(IUserDao.class);
//6.使用代理对象执行查询所有方法
List<User> users = userDao.findAll();
for(User user : users) {
	System.out.println(user);
}
//7.释放资源
session.close();
in.close();
```

#### 注意事项：

不要忘记在映射配置中告知mybatis要封装到哪个实体类中

配置的方式：指定实体类的全限定类名

#### mybatis基于注解的入门案例：

把IUserDao.xml移除，在dao接口的方法上使用@Select注解，并且指定SQL语句

```java
public interface IUserDao {
/**
* 查询所有用户
* @return
*/
	@Select("select * from user")
	List<User> findAll();
}
```

同时需要在SqlMapConfig.xml中的mapper配置时，使用class属性指定dao接口的全限定类名。

```xml
<!-- 告知 mybatis 映射配置的位置 -->
<mappers>
<mapper class="com.itheima.dao.IUserDao"/>
</mappers>
```

最后移除IUserDao.xml

### 明确：

我们在实际开发中，都是越简便越好，所以都是采用不写dao实现类的方式。

不管使用XML还是注解配置。

但是Mybatis它是支持写dao实现类的。

## 6、Mybatis底层的分析

### mybatis在使用代理dao的方式实现增删改查时做什么事呢？

只有两件事：

第一：创建代理对象

第二：在代理对象中调用selectList

**分析流程**

读入SqlMapConifg.xml的数据库连接信息，用来创建**Connection对象**

```xml
<!-- 配置连接数据库的信息：用的是数据源(连接池) -->
<dataSource type="POOLED">
	<property name="driver" value="com.mysql.jdbc.Driver"/>
	<property name="url" value="jdbc:mysql://localhost:3306/ee50"/>
	<property name="username" value="root"/>
	<property name="password" value="1234"/>
</dataSource>
```

获取映射配置信息

```xml
<!-- 告知 mybatis 映射配置的位置 -->
<mappers>
	<mapper resource="com/itheima/dao/IUserDao.xml"/>
</mappers>
```

读入IUserDao.xml，来获取SQL语句，**PreparedStatement**，和封装的实体类全限定名

```xml
<mapper namespace="com.itheima.dao.IUserDao">
	<!-- 配置查询所有操作 -->
	<select id="findAll" resultType="com.itheima.domain.User">
		select * from user
	</select>
</mapper>
```

**解析上面的配置文件，比如dom4j解析XML技术**

要想让下面的伪代码执行，我们需要给方法提供两个信息：

- 连接信息

- 映射信息

  - 执行的SQL语句

  - 封装结果的实体类全限定类名

    把这两个信息组合起来定义成一个对象**Mapper**，用于封装执行的SQL语句和结果类型的全限定类名

    | Key(String类型)      | Value()                |
    | -------------------- | ---------------------- |
    | com.itheima          | String sql             |
    | dao.IUserDao.findAll | String domainClassPath |

```
1.根据配置文件的信息创建Connection对象
	注册驱动，获取连接
2.获取预处理对象PreparedSatement
	此时需要SQL语句
	conn.prepareStatement(sql);
3.执行查询
	ResultSet resultSet=preparedStatement.executeQuery();
4.遍历结果集用于封装
	List<E> list = new ArrayList();
	while(resultSet.next()){
		E element=(E)Class.forName(配置的全限定类名).newInstance();//根据全限定名反射IUserDao
		实体类属性和表中的列名是一致的
		进行封装，把每个rs的内容都添加到element中
		把element加入到list中
		list.add(element);
	}
5.return list
```

```java
//5.使用 SqlSession 创建 dao 接口的代理对象
IUserDao userDao = session.getMapper(IUserDao.class);
//根据DAO接口的字节码创建DAO的代理对象
public <T> T getMapper(Class <T> daoInterfaceClass){
	/*
	*类加载器：它使用的和被代理对象是相同的类加载器
	*代理对象要实现的接口：和被代理对象实现相同接口
	*如何代理：它是增强的方法，我们需要自己来提供。此处是一个InvocationHandler的接口，我们需要写一个该接口的实现类在实现类中调用selectList方法
	*/
	Proxy.newProxyInstance(类加载器，代理对象要实现的接口字节码数组，如何代理);
}
```

### 自定义mybatis能通过入门案例看到类

class Resources **使用类加载器读取配置文件的类**

class SqlSessionFactoryBuilder

interface SqlSessionFactory

interface SqlSession

**首先需要将工具类引入到项目中**

```java
/**
* @author 黑马程序员
* @Company http://www.ithiema.com
* 用于解析配置文件
*/
public class XMLConfigBuilder {
 /**
 * 解析主配置文件，把里面的内容填充到 DefaultSqlSession 所需要的地方
 * 使用的技术：
 * dom4j+xpath
 * @param session
 */
public static void loadConfiguration(DefaultSqlSession session,InputStream
config){
}
/**
 * 根据传入的参数，解析 XML，并且封装到 Map 中
 * @param mapperPath 映射配置文件的位置
 * @return map 中包含了获取的唯一标识（key 是由 dao 的全限定类名和方法名组成）
 * 以及执行所需的必要信息（value 是一个 Mapper 对象，里面存放的是执行的 SQL 语句和
要封装的实体类全限定类名）
*/
private static Map<String,Mapper> loadMapperConfiguration(String
mapperPath)throws IOException {
}
/**
 * 根据传入的参数，得到 dao 中所有被 select 注解标注的方法。
 * 根据方法名称和类名，以及方法上注解 value 属性的值，组成 Mapper 的必要信息
 * @param daoClassPath
 * @return
 */
private static Map<String,Mapper> loadMapperAnnotation(String daoClassPath)throws Exception{
}
/**
* @author 黑马程序员
* @Company http://www.ithiema.com
* 负责执行 SQL 语句，并且封装结果集
*/
public class Executor {
}
/**
*
* <p>Title: DataSourceUtil</p>
* <p>Description: 数据源的工具类</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class DataSourceUtil {
}

```

**接着需要读取配置文件类**

```java
/**
*
* <p>Title: Resources</p>
* <p>Description: 用于读取配置文件的类</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class Resources {
	/**
	* 用于加载 xml 文件，并且得到一个流对象
	* @param xmlPath
	* @return
	* 在实际开发中读取配置文件:
	* 第一：使用类加载器。但是有要求：a 文件不能过大。 b 文件必须在类路径下(classpath)
	* 第二：使用 ServletContext 的 getRealPath()
	*/
	public static InputStream getResourceAsStream(String xmlPath) {
		return Resources.class.getClassLoader().getResourceAsStream(xmlPath);
	}
}
```

编写Mapper类

```java
/**
*
* <p>Title: Mapper</p>
* <p>Description: 用于封装查询时的必要信息：要执行的 SQL 语句和实体类的全限定类名</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class Mapper {
	private String queryString;//sql
	private String resultType;//结果类型的全限定类名
	public String getQueryString() {
		return queryString;
	}
	public void setQueryString(String queryString) {
		this.queryString = queryString;
	}
	public String getResultType() {
		return resultType;
	}
	public void setResultType(String resultType) {
		this.resultType = resultType;
	}
}

```

编写Configuration配置类

```
/**
* 核心配置类
* 1.数据库信息
* 2.sql 的 map 集合
*/
public class Configuration {
	private String username; //用户名
	private String password;//密码
	private String url;//地址
	private String driver;//驱动
	//map 集合 Map<唯一标识，Mapper> 用于保存映射文件中的 sql 标识及 sql 语句
	private Map<String,Mapper> mappers;
	public String getUsername() {
		return username;
	}
	public void setUsername(String username) {
		this.username = username;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public String getUrl() {
		return url;
	}
	public void setUrl(String url) {
		this.url = url;
	}
	public String getDriver() {
		return driver;
	}
	public void setDriver(String driver) {
		this.driver = driver;
	}
	public Map<String, Mapper> getMappers() {
		return mappers;
	}
	public void setMappers(Map<String, Mapper> mappers) {
		this.mappers = mappers;
	}
}
```

编写User实体类

```java
public class User implements Serializable {
	private int id;
	private String username;// 用户姓名
	private String sex;// 性别
	private Date birthday;// 生日
	private String address;// 地址
	//省略 getter 与 setter
	@Override
	public String toString() {
	return "User [id=" + id + ", username=" + username + ", sex=" + sex
+ ", birthday=" + birthday + ", address=" + address + "]";
	}
}
```

### 下面是基于XML的自定义mybatis框架

首先编写持久层接口和IUserDao.xml

**编写构建者类**

```java
/**
*
* <p>Title: SqlSessionFactoryBuilder</p>
* <p>Description: 用于构建 SqlSessionFactory 的</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class SqlSessionFactoryBuilder {
	/**
	* 根据传入的流，实现对 SqlSessionFactory 的创建
	* @param in 它就是 SqlMapConfig.xml 的配置以及里面包含的 IUserDao.xml 的配置
	* @return
	*/
	public SqlSessionFactoryBuilder(InputStream in) {
		DefaultSqlSessionFactory factory = new DefaultSqlSessionFactory();
		//给 factory 中 config 赋值
		factory.setConfig(in);
		return factory;
	}
}
```

**编写 SqlSessionFactory 接口和实现类**

```java
/**
*
* <p>Title: SqlSessionFactory</p>
* <p>Description: SqlSessionFactory 的接口</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public interface SqlSessionFactory {
	/**
	* 创建一个新的 SqlSession 对象
	* @return
	*/
	SqlSession openSession();
}
```

```java
/**
*
* <p>Title: DefaultSqlSessionFactory</p>
* <p>Description:SqlSessionFactory 的默认实现 </p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class DefaultSqlSessionFactory implements SqlSessionFactory {
	private InputStream config = null;
	public void setConfig(InputStream config) {
		this.config = config;
	}
	@Override
	public SqlSession openSession() {
		DefaultSqlSession session = new DefaultSqlSession();
		//调用工具类解析 xml 文件
		XMLConfigBuilder.loadConfiguration(session, config);
		return session;
	}
}
```

**编写 SqlSession 接口和实现类**

```java
/**
*
* <p>Title: SqlSession</p>
* <p>Description: 操作数据库的核心对象</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public interface SqlSession {
	/**
	* 创建 Dao 接口的代理对象
	* @param daoClass
	* @return
	*/
	<T> T getMapper(Class<T> daoClass);
	/**
	* 释放资源
	*/
	void close();
}
```

```java
/**
*
* <p>Title: DefaultSqlSession</p>
* <p>Description: SqlSession 的具体实现</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class DefaultSqlSession implements SqlSession {
	//核心配置对象
	private Configuration cfg;
	public void setCfg(Configuration cfg) {
		this.cfg = cfg;
	}
	//连接对象
	private Connection conn;
	//调用 DataSourceUtils 工具类获取连接
	public Connection getConn() {
		try {
			conn = DataSourceUtil.getDataSource(cfg).getConnection();
			return conn;
		} catch (Exception e) {
		throw new RuntimeException(e);
		}
	}
	/**
	* 动态代理：
	* 涉及的类：Proxy
	* 使用的方法：newProxyInstance
	* 方法的参数：
	* ClassLoader：和被代理对象使用相同的类加载器,通常都是固定的
	* Class[]：代理对象和被代理对象要求有相同的行为。（具有相同的方法）
	* InvocationHandler：如何代理。需要我们自己提供的增强部分的代码
	*/
	@Override
	public <T> T getMapper(Class<T> daoClass) {
		conn = getConn();
		System.out.println(conn);
		T daoProxy = (T) Proxy.newProxyInstance(daoClass.getClassLoader(),new
Class[] {daoClass}, new MapperProxyFactory(cfg.getMappers(),conn));
		return daoProxy;
	}
	//释放资源
	@Override
	public void close() {
	try {
		System.out.println(conn);
		conn.close();
	} catch (SQLException e) {
			e.printStackTrace();
		}
	}
	//查询所有方法
	public <E> List<E> selectList(String statement){
		Mapper mapper = cfg.getMappers().get(statement);
		return new Executor().selectList(mapper,conn);
	}
}
```

**编写用于创建 Dao 接口代理对象的类**

```java
/**
*
* <p>Title: MapperProxyFactory</p>
* <p>Description: 用于创建代理对象是增强方法</p>
* <p>Company: http://www.itheima.com/ </p>
*/
public class MapperProxyFactory implements InvocationHandler {
	private Map<String,Mapper> mappers;
	private Connection conn;
	public MapperProxyFactory(Map<String, Mapper> mappers,Connection conn) {
		this.mappers = mappers;
		this.conn = conn;
	}
	/**
	* 对当前正在执行的方法进行增强
	* 取出当前执行的方法名称
	* 取出当前执行的方法所在类
	* 拼接成 key
	* 去 Map 中获取 Value（Mapper)
	* 使用工具类 Executor 的 selectList 方法
	*/
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable
	{
		//1.取出方法名
		String methodName = method.getName();
		//2.取出方法所在类名
		String className = method.getDeclaringClass().getName();
		//3.拼接成 Key
		String key = className+"."+methodName;
		//4.使用 key 取出 mapper
		Mapper mapper = mappers.get(key);
		if(mapper == null) {
			throw new IllegalArgumentException("传入的参数有误，无法获取执行的必要条件");
		}
		//5.创建 Executor 对象
		Executor executor = new Executor();
		return executor.selectList(mapper, conn);
	}
}
```


---
icon: edit
date: 2022-09-13
tag:
	- Spring
category:
  - 学习
  - 生活
star: true
sidebarDepth: 2
---

# Spring

## 简介

### 什么是Spring?
分层的Java EE应用full-stack轻量级开源框架,以IOC和AOP为内核

IOC (Inverse Of Control:反转控制)
AOP (Aspect Oriented Programming:面向切面编程)

提供了展现层 SpringMVC和持久层 Spring JDBCTemplate 以及业务层事务管理等,整合了许多著名第三方框架.

### 发展历程
EJB1.0
EJB1.0
EJB1.0
EJB1.0
EJB1.0
EJB1.0

### 优势是什么?
1. 方便解耦,简化开发
2. AOP编程的支持
3. 声明式事务的支持
4. 方便程序的测试
5. 方便集成其他框架
6. 降低Java EE API的使用难度
7. Java 源码是经典学习范例



- 方便解耦，简化开发
通过Spring提供的IoC容器，我们可以将对象之间的依赖关系交由Spring进行控制，避免硬编码所
造成的过度程序耦合。有了Spring，用户不必再为单实例模式类、属性文件解析等这些很底层的需
求编写代码，可以更专注于上层的应用。

- AOP编程的支持
通过Spring提供的AOP功能，方便进行面向切面的编程，许多不容易用传统OOP实现的功能可以通过
AOP轻松应付。

- 声明事物的支持
在Spring中，我们可以从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活地进行事务的
管理，提高开发效率和质量。

- 方便程序的测试
可以用非容器依赖的编程方式进行几乎所有的测试工作，在Spring里，测试不再是昂贵的操作，
而是随手可做的事情。例如：Spring对Junit4支持，可以通过注解方便的测试Spring程序。

- 方便集成各种优秀框架
Spring不排斥各种优秀的开源框架，相反，Spring可以降低各种框架的使用难度，Spring提供了
对各种优秀框架（如Struts,Hibernate、Hessian、Quartz）等的直接支持。

- 降低Java EE API的使用难度
Spring对很多难用的Java EE API（如JDBC，JavaMail，远程调用等）提供了一个薄薄的封装层，
通过Spring的简易封装，这些Java EE API的使用难度大为降低。

- Java 源码是经典学习范例
Spring的源码设计精妙、结构清晰、匠心独用，处处体现着大师对Java设计模式灵活运用以及对
Java技术的高深造诣。Spring框架源码无疑是Java技术的最佳实践范例。如果想在短时间内迅速
提高自己的Java技术水平和应用开发水平，学习和研究Spring源码将会使你收到意想不到的效果。

### 体系结构
![[Pasted image 20220904152729.png]]

## Spring快速入门
### 开发步骤
创建对象时,通过`getbean(id)`方法获取所需的对象,
`getbean(id)`通过读取`XML`文件以及id来获得对象的全名,返回对应对象来完成对象创建.`XML`配置文件中的内容指向目的对象类.

解耦就是改变类时无序更改全部代码,只需更改`XML`中配置文件指向内容即可.

1. 导入Spring开发基本包坐标
2. 编写Dao接口和实现类
3. 创建Spring核心配置文件
4. 在Spring配置文件中配置`UserDaompl`
5. 使用Spring的API获得Bean实例

简化后就是:

1. 导入坐标
2. 创建Bean
3. 创建`applicationContext.xml`
4. 在配置文件中进行配置
5. 创建`ApplicationContext`对象`geiBean`


## 配置文件详解

```xml
<bean id=" " class = "" scope=" " init-method=" " destroy-method=" "><\bean>
```
#### `Bean`标签基本配置

`id` Bean实例在`Spring`容器中的唯一标识  
`class` Bean的全限定名称

#### `Bean`标签的范围配置

`scope`:指对象的作用范围,取值如下:
`singleton` 默认值,单例的
`prototype` 多例的

`singleton` 默认值,单例的
`Bean`的实例化个数: 1
`Bean`的实例化时机: 当`Spring`核心文件被加载时,实例化配置的`Bean`实例
`Bean`的生命周期: 
1. 对象创建: 当应用加载,创建容器时,对象就被创建了.(应用初始化时创建)
2. 对象运行: 只要容器在,对象一直活着.
3. 对象销毁:当应用卸载,销毁容器时,对象就被销毁了.(与容器共存亡,容器创建,`Bean`创建,容器销毁,`Bean`销毁)

`prototype` 多例的
`Bean`的实例化个数: 多个
`Bean`的实例化时机: 当调用`getBean()`方法时实例化`Bean`
`Bean`的生命周期: 
1. 对象创建:当使用对象时,创建新的对象实例.(使用时创建)
2. 对象运行:只要对象在使用中,就一直活着.
3. 对象销毁:当对象长时间不用时,被`Java`的垃圾回收机制回收了.

#### `Bean`生命周期配置

`init-method`:指定类中的初始化方法名称
`destroy-method`:指定类中销毁方法名称

#### `Bean`实例化三种方式

- 无参*构造*方法实例化
- 工厂*静态*方法实例化
- 工厂*实例*方法实例化

无参构造方法实例化

工厂*静态*方法实例化
```xml
//XML文件配置
<bean id = "noCanCreate" class="com.taytay.factory.StaticFactory" factory-method="getUserDao"> </bean>
```
//StaticFactory.java配置
```java
public class StaticFactory {  
    public static UserDao getUserDao(){  
        return new UserDaoImpl();  
    }  
}
```


工厂*实例*方法实例化
//XML文件配置
```xml
<bean id="factory" class="com.taytay.factory.DynamicFactory"> </bean>  
<bean id="userDao" factory-bean="factory" factory-method="getUserDao"> </bean>
```
//DynamicFactory.java配置
```java
public class DynamicFactory {  
    public UserDao getUserDao(){  
        return new UserDaoImpl();  
    }  
}
```


#### `Bean`依赖注入
举例,因为`UserService`和`UserDao`都在`Spring`容器中,需要使用`UserService`类和`UserDao`的方法,可以在容器中将`UserDao`注入`UserService`,然后再获取`UserService`类进行使用.

不提供依赖注入时,是将`UserService`和`UserDao`在程序代码中进行组装,能否直接在`Spring`容器中就组装呢?因为实际操作的只有`UserService`.

提供依赖注入后,只需要获取`UserService`实例,再通过这个实例调用`UserDao`的方法.

依赖注入(`Dependency Injection`): 是`Spring`框架核心`IOC`的具体实现
降低耦合度,但并不是不需要耦合.主要是降低它们之间的依赖程度.

所以要怎么在容器中组装呢?

#### `Bean`依赖注入的注入方式
- 构造方法
- `set`方法

构造方法
```Java
public class UserServiceImpl implements UserService{
	private UserDao userDao;
	public void save(){  
	    userDao.save();  
	    System.out.println("save running");  
	}  
	public UserServiceImpl(UserDao userDao){  
	    this.userDao = userDao;  
	}  
	public UserServiceImpl(){  
  
	}

}	
```

```xml
<bean id="userService" class="com.service.impl.UserServiceImpl">   
    <constructor-arg name="userDao" ref="userDao"></constructor-arg>  
</bean>
```

`set`方法
```java
public class UserServiceImpl implements UserService{

	private UserDao userDao;
	public void setUserDao(UserDao userDao){
		this.userDao = userDao;
	}
	public void save(){
		userDao.save();//使用userDao的save方法;
	}
}
```

```xml
<bean id = "userDao" class = "com.dao.UserDaoImpl"> </bean>
<bean id = "userService" class = "com.service.UserServiceImpl">
	<property name = "userDao" ref = "userDao"> </property>
<\bean>
```


p命名空间注入
本质也是set注入,不过比直接set注入更方便.主要用于注入过多的时候.

首先引入p命名空间
```xml
<xmlns:p="http://www.springframework.org/schema/p">
```
也要修改注入方式
```xml
<bean id = "UserService" class = "com.service.UserService" p:userDao-ref = "userDao"/>
```

#### `Bean`的依赖注入的数据类型
前边都是注入引用的`Bean`,除了对象的引用可以注入,普通数据类型,集合等都可以在容器中进行注入.为什么要设定这种呢?因为有时候只需要对象的其中几个变量,无需全部注入.

注入数据的三种数据类型:
- 普通数据类型
- 引用数据类型
- 集合数据类型

普通数据类型:
```xml
<bean id="userDao" class="com.taytay.dao.impl.UserDaoImpl">
<property name="username" value="张三"/>
<property name="age" value="18"/>
</bean>
```
引用数据类型:
```XML
here I am 
```




### Spring配置文件引入
实际开发中,`Spring`的配置内容非常多,这就导致`Spring`的配置文件非常的臃肿,所以可以通过拆解的方式将不同模块的内容分到不同的配置文件中,这种用法为:
```XML
<import resource="applicationContext-xxx.xml"/>
```


### Spring的重点配置
```XML
<bean>
	id 属性:容器中Bean的唯一标识
	class 属性:要实例化的Bean的全限定名
	scope 属性:作用范围,是单例还是多例
	<property>
		name 属性:属性名称
		value 属性:注入的普通属性值
		ref 属性:注入的对象引用值
		<list>标签
		<map>标签
		<properties>标签
		<constructor-arg>标签 构造方法注入
<import> 导入其他的配置文件
```

## Spring相关API


### ApplicationContextd的继承体系
接口类型,代表应用上下文,可以通过其实例获得`Spring`容器中的`Bean`对象.


`ClassPathApplicationContext`
从类的根路径下加载配置文件

`FileSystemXmlApplicationContext`
文件系统地址,从文件磁盘地址获取配置文件

`AnnotationConfigApplicationContext`
使用注解配置容器对象时,需要此类来创建`Spring`容器,用其读取注解

### `getBean`方法

```Java
public Object getBean(String name) throws BeansException{
	assertBeanFactoryActive();
	return getBeanFactory.getBean(name);
}
public <T> T getBean(Class<T> requiredType) throws BeanException{
	assertBeanFactoryActive();
	return getBeanFactory.getBean(requiredType);
}
```

```java
ApplicationContext app = new ClasspathXmlContext("xml文件")
app.getBean(id)
```

## Spring配置数据源

### 数据源(连接池)作用






### 数据源开发步骤



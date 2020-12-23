**2020.10.08**

**包名：com.atlgq.spring.beans.factory**

利用静态工厂方法：直接调用某一个类的静态方法就可以返回Bean的实例

实例工厂的方法，即先需要创建工厂本身再调用工厂的实例方法来返回bean实例

**包名：com.atlgq.spring.beans.factorybean**

自定义factorybean要先实现FactoryBean接口

在xml文件中:	通过factorybean来配置bean的实例

​	class：指向factorybean的全类名

​	property：配置factorybean的属性

**包名：com.atlgq.spring.beans.annotation.\***

spring通过注解配置bean，可以通过@方式去注解，然后使用UserRepository去

包含进来，其中可以使用只包含（include-filter）和除非（exclude-filter）两种方式，

只包含要和（use-default-filters="false"）配合使用，注解可以自动装配，使用@autowire就可以，如果有相同的bean，那么是可以通过@Qualifier去指定的，也可以使用@Repository("userRepository")直接赋值的方法

**包名:com.atlgq.spring.aop.impl**

1.spring aop

1)、加入jar包：

com.springsource.org.aopalliance-1.0.0.jar

com.springsource.org.aspectj.weaver-1.6.4.RELEASE.jar

commons-logging-1.2.jar

spring-aop-4.0.6.RELEASE.jar

spring-aspects-4.0.6.RELEASE.jar

spring-beans-4.0.6.RELEASE.jar

spring-context-4.0.6.RELEASE.jar

spring-core-4.0.6.RELEASE.jar

spring-expression-4.0.6.RELEASE.jar

2）、在配置加入aop的命名空间

3）、基于注解的方式

①、在配置文件中加入如下配置：

<!-- 使用Aspject注解起作用：自动匹配的类生成代理对象 -->

​	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>

②、把横切关注点的代码抽象到切面的类中

i、切面首先是一个IOC的bean，即加入@Component注解

ii、切面还需要加入@Aspect

③、在类中声明各种通知：

i、声明一个方法

ii、在方法前加入一个@Before注解

④、可以在通知方法中声明一个类型为JoinPoint的参数，然后就能访问链接细节，	

如方法名称和参数值

包名：com.atlgq.spring.aop

aop使用

1、确定好类

2、利用@Component和配置

bean（<context:component-scan base-package="com.atlgq.spring.aop">

 </context:component-scan>）引入IOC容器

3、新建类，设置@Aspect设置切面，利用@Before等设置前置通知，后置通知等通知

4、在xml配置

<!-- 配置自动未匹配aspect注解的java类生成代理对象 -->

​	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>

5、在Main引用。

6、可以利用@Order()确定切面的优先等级，值越小，等级越大

**2020.10.10**

**包名：com.atlgq.spring.aop.xml**

切点表达式bean配置

<!-- 配置AOP -->

​	<aop:config>

​		<!-- 配置切点表达式 -->

​		<aop:pointcut expression="execution(* 		com.atlgq.spring.aop.xml.ArithmeticCalculator.*(..))" id="pointcut"/>

​		<!-- 配置切面及通知 -->

​		<aop:aspect ref="loggingAspect" order="2">

​			<aop:before method="beforeMethod" pointcut-ref="pointcut"/>

​			<aop:after method="afterMethod" pointcut-ref="pointcut"/>

​			<aop:after-throwing method="afterThrowing" pointcut-ref="pointcut" throwing="e"/>

​			<!-- <aop:around method="aroundMethod" pointcut-ref="pointcut"/> -->

​		</aop:aspect>

​		

​		<aop:aspect ref="vlidationAspect" order="1">

​			<aop:before method="validateArgs" pointcut-ref="pointcut"/>

​		</aop:aspect>

​	</aop:config>

包名：com.atlgq.spring.jdbc

利用JdbcTemplate和NamedParameterJdbcTemplate可以实现mysql的链接操作，JdbcTemplate使用问好，得去一一对应，NamedParameterJdbcTemplate的话，可以使用使用具名参数去传入，也可以传入对象直接赋值。各有优缺点

**2020.10.14**

**包名：com.atlgq.spring.tx**

通过注解的方式实现:

当图书馆里系统出现余额不足但是本书依旧出现问题的时候，可以采用事物管理的方式，首先在配置文件里面

​	<!-- 配置事务管理器 -->

​	<bean id="transactionManager" 

​		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">

​		<property name="dataSource" ref="dataSource"></property>	

​	</bean>

​	

​	<!-- 启用事务注解 -->

​	<tx:annotation-driven transaction-manager="transactionManager"/> 

然后在方法的前面使用@Transactional就可以解决以上问题。

//添加事务注解

​	//1.使用propagation指定事务的传播行为，即当前事物方法被另外一个事物方法所调用是

​	//如何使用事务，默认情况下取值为REQUIRED，即取用调用方法的事务。

​	//160元去买价值100和70的书，钱不够，就取消购买。

​	//REQUIRED_NEW：使用自己的事务，调用的事务的方法的事务被吊起。

​	////160元去买价值100和70的书，钱不够，就购买第一本。

​	//2.使用isolation，指定事务的隔离级别，最常用的取值为Isolation.READ_COMMITTED

​	//3.默认情况下Spring 的声明式事务对所有运行时异常进行回滚，也可以通过对对应的属性进行			 设置,通常情况下取默认值就可以

​	//4.@Transactional(propagation=Propagation.REQUIRED,isolation=Isolation.READ_COMMITTED,noRollbackFor={UserAccountException.class})

​	//事务的回滚属性，指定事务是否回滚

​	//5.readOnly=false事务是否只读

​	//6.使用timeout指定强制回滚之前事务可以占用的时间,如果3秒之内还没有完成强制回滚。

包名：com.atlgq.spring.xml.*

通过xml文件的方式

1.配置事务管理器

2.配置事务属性

3.配置事务切入点，以及把事务切入点和事务属性关联
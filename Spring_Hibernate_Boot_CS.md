# Spring/SpringBoot
- All Annotations: https://github.com/jdbirla/JD_spring-master-class/blob/master/Spring_Annotatoin_world.adoc

## Spring

### Bean Scopes
singleton - One instance per Spring Context
prototype - New bean whenever requested
request - One bean per HTTP request. Web-aware
Spring ApplicationContext.
session - One bean per HTTP session. Web-aware
Spring ApplicationContext.

- when bean in static scope and dependecy in prototype even though dependecy classs will the same object but if we want to get different objects for dependcy class then we need to use proxymode where bean wll ge the same object but dependcy bean will get new object when request using proxy
![image](https://user-images.githubusercontent.com/69948118/202094968-103258ef-5f15-46a4-8fb2-68eeac8b38be.png)

### Application Contexts
```java
ApplicationContext context =
new ClassPathXmlApplicationContext(
new String[] {"BusinessApplicationContext.xml",
"Other-Configuration.xml"});

@Configuration
class SpringContext {
}
ApplicationContext ctx =
new AnnotationConfigApplicationContext(
SpringContext.class);
```
### Autowiring
byType
byName
constructor - similar to byType, but through
constuctor

### by Type - Class or Interface

```java

@Component
public class ComplexAlgorithmImpl {
@Autowired
private SortAlgorithm sortAlgorithm;
public interface SortAlgorithm {
public int[] sort(int[] numbers);
}
@Component
public class QuickSortAlgorithm implements SortAlgorithm {

```

###  by Name
```java
@Component
public class ComplexAlgorithmImpl {
@Autowired
private SortAlgorithm quickSortAlgorithm;
public interface SortAlgorithm {
public int[] sort(int[] numbers);
}
@Component
public class QuickSortAlgorithm implements SortAlgorithm {
@Component
public class BubbleSortAlgorithm implements SortAlgorithm {
```

### GOF vs Spring bean scope
- GOF will give single obhect per JVM but spring bean scope will give single object per Application Context in under same JVM

### Read values from external properties file
![image](https://user-images.githubusercontent.com/69948118/202136762-71cb9438-79ae-4fed-9b97-c789c77553ff.png)
![image](https://user-images.githubusercontent.com/69948118/202136988-8041c551-596c-4af1-82ff-8e16bdfaf6b9.png)

### Spring AOP (Basic interception for spring managed beans)
```java
@Aspect
@Configuration
public class UseAccessAspect {
	
	private Logger logger = LoggerFactory.getLogger(this.getClass());
	
	//What kind of method calls I would intercept
	//execution(* PACKAGE.*.*(..)) : pointcuts
	
	@Before("execution(* com.in28minutes.spring.aop.springaop.business.*.*(..))")
	public void before(JoinPoint joinPoint){
		logger.info(" Check for user access ");
		logger.info(" Allowed execution for {}", joinPoint);
	}
}
```
![image](https://user-images.githubusercontent.com/69948118/202164677-cecb4b24-5f1b-43e9-b8fb-b89d52f9307e.png)
![image](https://user-images.githubusercontent.com/69948118/202164716-cb26d040-d09e-438d-a945-5fe120798e74.png)
![image](https://user-images.githubusercontent.com/69948118/202166064-3ad20608-b6fb-48cf-a2d6-3e5550d5022e.png)
![image](https://user-images.githubusercontent.com/69948118/202167003-c6f66363-1a75-47b3-822a-40672caab6f2.png)
![image](https://user-images.githubusercontent.com/69948118/202170123-11eb242f-b701-49e2-a5e2-0d7c9ae82e03.png)



### Aspectj (more powerful interception than spring AOP)

### Spring MVC
- Servlet and jsp using JEE
![image](https://user-images.githubusercontent.com/69948118/202369231-f2a17dc6-a9c2-4e7e-b61c-5b968c3f7df2.png)
- Spring MVC
![image](https://user-images.githubusercontent.com/69948118/202375026-40dd1541-391f-4e02-91fc-62175ddf6fa7.png)
![image](https://user-images.githubusercontent.com/69948118/202388675-1079bd32-9382-49b9-b7f8-bd022def6ab5.png)
![image](https://user-images.githubusercontent.com/69948118/202388900-271db70d-ef76-4424-b545-592972fc8259.png)
![image](https://user-images.githubusercontent.com/69948118/202393126-c5abfcb9-041f-4863-b8c7-ba48a687fe10.png)




---
## Spring Boot
### Spring Boot with MVC

- /src/main/resources/META-INF/resources/WEB-INF/jsp/sayHello.jsp
```html
<html>
	<head>
		<title> My first HTML Page - JSP</title>
	</head>
	<body>
		My first html page with body - JSP
	</body>
</html>
```
- /src/main/resources/application.properties Modified here you can see no need of dispatcher servlet as spring boot is doing configuration for us
```properties
spring.mvc.view.prefix=/WEB-INF/jsp/
spring.mvc.view.suffix=.jsp
logging.level.org.springframework=debug**
```
#### Spring boot logging
![image](https://user-images.githubusercontent.com/69948118/202606001-2f18f2d0-ccd0-4623-a48a-3c91abec1125.png)

### Request vs Modle Vs Session
- Model is also valid till that request 
@SessionAttribultes ("name")
![image](https://user-images.githubusercontent.com/69948118/202609457-ed31de58-eae2-4906-a86f-e4d339de7c88.png)

#### web jars https://www.webjars.org/
- client side libraries(jQuery , bootstrap) in Java jar packaging

#### Validation with Spring Boot
![image](https://user-images.githubusercontent.com/69948118/202620125-2b720608-1426-43e5-8dd2-8912206eebc5.png)
![image](https://user-images.githubusercontent.com/69948118/202621622-d6e1a8e3-0c0e-4345-b735-bb86c98fe80e.png)
![image](https://user-images.githubusercontent.com/69948118/202624247-409bcb1d-c3f0-4a9c-8d0f-b7bf3ffd07d7.png)

### Spring Security
```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
### Spring Data
![image](https://user-images.githubusercontent.com/69948118/202648920-e786dd82-b48d-41d4-839b-10da8df2c955.png)

### Spring Boot REST
- @restController is combination of @controller and @ResponseBody
- When will return any string it will be return as string 
- When will return any bean it will return as JSON of the bean because of starter project and autconfiguration of Jackson
- 

#### Model Attribute
Indicates the purpose of that method is to add one
or more model attributes.
Invoked before @RequestMapping methods.
Used to fill the model with commonly needed
attributes
Drop down values for form
```java
@ModelAttribute
public void addAttributes(Model model) {
model.addAttribute("options",
Arrays.asList(
"Option 1","Option 2","Option 3" ));
}
```
---



---

## AOP

```java
@Aspect
@Component
class MyAspect {
@Before("execution(* HiByeService.*(..))")
public void before(JoinPoint joinPoint) {
System.out.print("Before ");
System.out.print(joinPoint.getSignature().getName());
System.out.println(Arrays.toString(joinPoint.getArgs()));

@AfterReturning(pointcut = "execution(* HiByeService.*(..))"
, returning = "result")
public void after(JoinPoint joinPoint, Object result) {
System.out.print("After ");
System.out.print(joinPoint.getSignature().getName());
System.out.println(" result is " + result);
}
}
}

@Around(value = "execution(* HiByeService.*(..))")
public void around(ProceedingJoinPoint joinPoint)
throws Throwable {
long startTime = new Date().getTime();
Object result = joinPoint.proceed();
long endTime = new Date().getTime();
System.out.print("Execution Time - "
+ (endTime - startTime));
}
```



```java

```


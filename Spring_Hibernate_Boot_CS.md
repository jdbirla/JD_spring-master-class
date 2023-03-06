# Spring/SpringBoot/Hibernate Cheat Sheet

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

## JDBC/Spring JDBC/JPA/Hibernate

### JDBC
```java
Connection connection = datasource.getConnection();
PreparedStatement st = connection.prepareStatement(
"Update todo set user=?, desc=?, target_date=?, is_done=? where id=?"
st.setString(1, todo.getUser());
st.setString(2, todo.getDesc());
st.setTimestamp(3, new Timestamp(todo.getTargetDate().getTime()));
st.setBoolean(4, todo.isDone());
st.setInt(5, todo.getId());
st.execute();
st.close();
connection.close();
```
### Spring JDBC
```java
jdbcTemplate
.update
("Update todo set user=?, desc=?, target_date=?, is_done=? where id=?"
todo.getUser(), todo.getDesc(),
new Timestamp(todo.getTargetDate().getTime()),
todo.isDone(), todo.getId());
jdbcTemplate.update("delete from todo where id=?",
id);
--------------------
new BeanPropertyRowMapper(Todo.class)
class TodoMapper implements RowMapper<Todo> {
@Override
public Todo mapRow(ResultSet rs, int rowNum)
throws SQLException {
Todo todo = new Todo();
todo.setId(rs.getInt("id"));
todo.setUser(rs.getString("user"));
todo.setDesc(rs.getString("desc"));
todo.setTargetDate(
rs.getTimestamp("target_date"));
todo.setDone(rs.getBoolean("is_done"));
return todo;
}
}
```

### JPA
```java
@Entity
@Table(name = "Todo")
public class Todo {
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private int id;
private String user;
private String desc;
private Date targetDate;
private boolean isDone;
-----------------------
public class TodoJPAService implements TodoDataService {
@PersistenceContext
private EntityManager entityManager;
@Override
public void updateTodo(Todo todo) {
entityManager.merge(todo);
}
```

### One-to_one
```java
@Entity
public class Passport {
....
// Inverse Relationship
// bi-directional OneToOne relationship
// Column will not be created in the table
// Try removing mappedBy = "passport" => You will see that student_// will be created in passport
@OneToOne(fetch = FetchType.LAZY, mappedBy = "passport")
private Student student;
@Entity
@Table(name = "Student")
public class Student {
@OneToOne
private Passport passport;

```

### One to Many Relationship
```java
@Entity
public class Project {
@OneToMany(mappedBy = "project")
private List<Task> tasks;
@Entity
public class Task {
@ManyToOne
@JoinColumn(name="PROJECT_ID")
private Project project;
```
### Many to Many Relationship
```java
@Entity
public class Project {
@ManyToMany
// @JoinTable(name="STUDENT_PROJ",
// joinColumns=@JoinColumn(name="STUDENT_ID"),
// inverseJoinColumns=@JoinColumn(name="PROJECT_ID"))
private List<Student> students;
public class Student {
@ManyToMany(mappedBy = "students")
private List<Project> projects;
```
### Defining a Data Source
```java
#HSQL in-memory db
db.driver=org.hsqldb.jdbcDriver
db.url=jdbc:hsqldb:mem:firstdb
db.username=sa
db.password=
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
destroy-method="close">
<property name="driverClass" value="${db.driver}" />
<property name="jdbcUrl" value="${db.url}" />
<property name="user" value="${db.username}" />
<property name="password" value="${db.password}" />
</bean>
```

### Configuring Hibernate
src\main\resources\config\hibernate.properties
```java
hibernate.dialect=org.hibernate.dialect.HSQLDialect
hibernate.show_sql=false
hibernate.format_sql=false
hibernate.use_sql_comments=true
```

### persistence.xml
```java
src\main\resources\META-INF\persistence.xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://java.sun.com/xml/ns/persistence"
version="2.0">
<persistence-unit name="hsql_pu" transaction-type="RESOURCE_LOCAL"
<provider>org.hibernate.jpa.HibernatePersistenceProvider</provider
<properties>
<property name="hibernate.dialect" value="org.hibernate.dialect.<property name="hibernate.connection.url" value="jdbc:hsqldb:<property name="hibernate.connection.driver_class" value="org.<property name="hibernate.connection.username" value="sa" />
<property name="hibernate.connection.password" value="" />
</properties>
</persistence-unit>
</persistence>
```

### Configure Entity Manager Factory and Transaction Manager
```java
<bean
class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"
id="entityManagerFactory">
<property name="persistenceUnitName" value="hsql_pu" />
<property name="dataSource" ref="dataSource" />
</bean>
<bean id="transactionManager" class="org.springframework.orm.jpa.<property name="entityManagerFactory" ref="entityManagerFactory"
<property name="dataSource" ref="dataSource" />
</bean>
<tx:annotation-driven transaction-manager="transactionManager"
```

### Making Service Transactional
```java
@Service
public class StudentService {
@Autowired
StudentRepository service;
@Transactional
public Student insertStudent(Student student) {
return service.insertStudent(student);
}
@Service
@Transactional
public class StudentService {
@Autowired
StudentRepository service;
```

### JDBC VS JDBC template VS JPA VS Hibernate  VS Spring Data JPA

|#  |JDBC|Spring JDBC template |JPA|Hibernate|Spring Data JPA|
|---|--- |---           |---|---      |---			  |
|Description|The very first way to connect a Java application to a database is using JDBC |Spring JdbcTemplate is a powerful mechanism to connect to the database and execute SQL queries. It internally uses JDBC api, but eliminates a lot of problems of JDBC API. | JPA is a specification and defines the way to manage relational database data using java objects.| Hibernate is an implementation of JPA. It is an ORM tool to persist java objects into the relational databases.|Spring Data JPA is a JPA data access abstraction. Spring Data JPA cannot work without a JPA provider |
|Syntax| ```Class.forName("com.mysql.jdbc.Driver");  Connection con=DriverManager.getConnection(  "jdbc:mysql://localhost:3306/sonoo","root","root");  //here sonoo is database name, root is username and password  Statement stmt=con.createStatement();  ResultSet rs=stmt.executeQuery("select * from emp");  while(rs.next())  System.out.println(rs.getInt(1)+"  "+rs.getString(2)+"  "+rs.getString(3));  con.close();  ```| ```int result = jdbcTemplate.queryForObject(     "SELECT COUNT(*) FROM EMPLOYEE", Integer.class); ```| ```  EntityManagerFactory factory = Persistence.createEntityManagerFactory("UsersDB");         EntityManager entityManager = factory.createEntityManager();                  entityManager.getTransaction().begin();                  User newUser = new User();         newUser.setEmail("billjoy@gmail.com");         newUser.setFullname("bill Joy");         newUser.setPassword("billi");                  entityManager.persist(newUser);                  entityManager.getTransaction().commit();                  entityManager.close();         factory.close();```| ```    StandardServiceRegistry ssr = new StandardServiceRegistryBuilder().configure("hibernate.cfg.xml").build();     Metadata meta = new MetadataSources(ssr).getMetadataBuilder().build();  	SessionFactory factory = meta.getSessionFactoryBuilder().build();  	Session session = factory.openSession();  	Transaction t = session.beginTransaction();                   Employee e1=new Employee();        e1.setId(101);        e1.setFirstName("Gaurav");        e1.setLastName("Chawla");                session.save(e1);      t.commit();      System.out.println("successfully saved");        factory.close();      session.close();    ```| ```public interface CustomerRepository extends CrudRepository<Customer, Long> {     List<Customer> findByLastName(String lastName); } ``` ```  private CustomerRepository repository;          public void test() {         // Save a new customer         Customer newCustomer = new Customer();         newCustomer.setFirstName("John");         newCustomer.setLastName("Smith");                  repository.save(newCustomer);                  // Find a customer by ID         Optional<Customer> result = repository.findById(1L);         result.ifPresent(customer -> System.out.println(customer));                  // Find customers by last name         List<Customer> customers = repository.findByLastName("Smith");         customers.forEach(customer -> System.out.println(customer));                  // List all customers         Iterable<Customer> iterator = repository.findAll();         iterator.forEach(customer -> System.out.println(customer));                  // Count number of customer         long count = repository.count();         System.out.println("Number of customers: " + count);```|


```java
public interface CustomerRepository extends CrudRepository<Customer, Long> {
	List<Customer> findByLastName(String lastName);
}

private CustomerRepository repository;

public void test() { // Save a new customer
	 Customer newCustomer = new Customer(); 
	newCustomer.setFirstName("John"); 
	newCustomer.setLastName("Smith"); 
	repository.save(newCustomer); 
	// Find a customer by ID 
	Optional<Customer> result = repository.findById(1L);
	result.ifPresent(customer -> System.out.println(customer)); 
	// Find customers by last name 
	List<Customer> customers = repository.findByLastName("Smith"); 
	customers.forEach(customer -> System.out.println(customer)); 
	// List all customers 
	Iterable<Customer> iterator = repository.findAll(); 
	iterator.forEach(customer -> System.out.println(customer)); 
	// Count number of customer 
	long count = repository.count();
	System.out.println("Number of customers: " + count);
}
```
---
###  JPA VS Hibernate
![image](https://user-images.githubusercontent.com/69948118/173209797-a8a5237d-4885-4aa8-b9a6-7651a9309449.png)
![image](https://user-images.githubusercontent.com/69948118/173209984-26b1dd2a-3929-4989-baf9-725c3b5eb5c9.png)
![image](https://user-images.githubusercontent.com/69948118/175798578-53560c85-0adc-4777-820e-5f654f25114a.png)
![image](https://user-images.githubusercontent.com/69948118/175798641-1ef19dee-9e6b-48a5-b378-fc2e42b5e218.png)
![image](https://user-images.githubusercontent.com/69948118/175798758-6b117595-b66d-4edd-a959-6d9944ab6078.png)
![image](https://user-images.githubusercontent.com/69948118/175798832-18ed5761-31e8-49e3-ac3e-0c719045b4d6.png)
![image](https://user-images.githubusercontent.com/69948118/175798846-67b5b6db-2d59-4b78-92c0-284555bfe4b0.png)
![image](https://user-images.githubusercontent.com/69948118/218351568-b014dd37-e601-4f34-9285-8f662363cc8f.png)

---

## Spring Data JPA
### Using Spring Data JPA
```java
public interface StudentRepository extends CrudRepository<Student
}
public interface PassportRepository extends CrudRepository<Passport
}
```

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


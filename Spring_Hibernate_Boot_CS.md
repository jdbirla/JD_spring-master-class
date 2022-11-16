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
### Model Attribute
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
### GOF vs Spring bean scope
- GOF will give single obhect per JVM but spring bean scope will give single object per Application Context in under same JVM

---
## Sprin Boot

### Spring Boot Features
Auto Configuration
Spring Boot Starter Projects
Spring Boot Actuator
Embedded Server

### Spring Boot
Eliminates all configuration needed by Spring and
Spring MVC and auto configures it
No need for @ComponentScan. Default
Component Scan.
No need to configure DispatcherServlet
No need to configure a Data Source, Entity
Manager Factory or Transaction Manager.

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


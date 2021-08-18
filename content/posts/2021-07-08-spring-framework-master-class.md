---
layout: post
title: "Spring Framework Master Class"
date: 2021-07-08T10:24:50-05:00
categories: [programming, spring, web, java]
draft: false
---

From <https://udemy.com/course/spring-tutorial-for-beginners/>.

Work for this  can be found at <https://github.com/HenryFBP/Spring-Framework-Master-Class>

Work from the instructor can be found at <https://github.com/in28minutes/spring-master-class/tree/master>

## Section 1. Intro to Spring

What is Spring?

A: It's a Dependency Injection Framework.

What is DI?

What is a dependency??

### Dependency

Below, the web layer depends on the business layer. The business layer depends on the data layer. Etc.

![](/images/2021-07-08-spring-framework-master-class/layers.png)

Typical code may have a service that is tightly coupled with some implementation of an algorithm, i.e.:

```java
public class BusinessService{

    SortAlgo sortAlgo = new BubbleSortAlgo();
}
```

```java
public class BubbleSortAlgo implements SortAlgo {
    ...
}
```

This is tight coupling because the `BusinessService` class is tightly coupled with a specific implementation of a class, `BubbleSortAlgo`. 

We can make this code loosely coupled by removing instantiation of dependencies:

```java
public class BusinessService{

    SortAlgo sortAlgo;  // = new BubbleSortAlgo();

    public BusinessService(SortAlgo sa){
        this.sortAlgo=sa;
    }
}
```

Now the `BusinessService` class can use any `SortAlgo` class it wants.

...

Who does this? Where do you write this logic?

...

This is where Spring Framework comes in.

SF does this. It instantiates objects and populates their dependencies.

It's my/your job as a programmer to tell Spring Framework what objects it needs to manage and what the dependencies of each class are.

- How does Spring know to create an instance of `BubbleSortAlgo`?
- How does Spring know to create an instance of `BusinessService` and populate it with an instance of `BubbleSortAlgo`?

i.e; how does Spring know to do this:

```java
SortAlgo sa = new BubbleSortAlgo();

BusinessService bs = new BusinessService(sa);
```

given this code:

```java
public class BusinessService{

    SortAlgo sortAlgo;

    public BusinessService(SortAlgo sa){
        this.sortAlgo=sa;
    }
}
```

There are 2 annotations that tell Spring what objects it needs to manage and what are the dependencies of a class.

## `@Component` and `@Autowired`, what is Dependency Injection?

```java
@Component
public class BusinessService {

    @Autowired
    SortAlgo sortAlgo;
}
```

```java
@Component
public class BubbleSortAlgo implements SortAlgo {...}
```

We are telling Spring
> "You need to start managing instances of `BusinessService` and `BubbleSortAlgo`"

But how do we know that `SortAlgo` is a dependency of `BusinessService`?

We use another annotation: `@Autowired`.

`@Component` will make Spring start managing instances of that class.

`@Autowired` means Spring will start looking for a SF (Spring Framework) managed class to find a matching class for the `sortAlgo` variable. Internally, Spring will assign a `new BubbleSortAlgo()` to that `sortAlgo` variable, by passing it as an argument to the constructor to a `BusinessService` object.

This is called Dependency Injection. SF makes sure that all SF managed classes are created with dependencies properly populated.

### Terminology

- Beans
    - In the above java code, we are telling SF to manage `BusinessService` and its dependencies (`SortAlgo`) by using annotations. Those two instances that SF manages are called "Beans".

    - Beans are the different objects that are managed by SF.

- Autowiring
    - The process of SF 

- Dependency Injection
    - At a high level, the `@Autowired` annotation causing the `SortAlgo` object to be populated is DI.
    - We are declaring X as a dependency of Y, and X gets automatically filled in.

You may notice these are all kind of related. And all sinple.

- Inversion of Control
    - When we started off this example, we were explicitly assigning a value to the `sortAlgo` variable. The class `BusinessService` was responsible for creating an instance of the `SortAlgo`.
    - But when we use `@Autowired`, we delegate control of creating the dependency instance to Spring Framework. We're taking the control away from the class that wants a dependency, and giving it to SF. This is what IoC is.

- IOC Container
- Application Context
    - The most important IOC Container as far as Spring is concerned is the AC.
    - The AC creates and manages all of the beans
    - This is the most important part of SF.

### What is Spring?

Spring is a DI Framework!

## Section 2: Spring Master Class - Level 1 to Level 6 - Course Overview, Github & More

### Spring Framework Master Class - Overview

- <https://github.com/in28minutes/spring-master-class>
- [Course Guide PDF](/files/2021-07-08-spring-framework-master-class/SpringMasterClass-CourseGuidev0.1.pdf)

You learn the basics of SF in 6 levels. See Course Guide.

## Section 3: Spring Level 1 - Introduction to Spring Framework in 10 Steps

### Section Introduction - Spring Framework in 10 Steps

See <https://github.com/in28minutes/spring-master-class/tree/master/01-spring-in-depth> for code.

Use Eclipse Oxygen. (or intellij idea, i do)

### Spring Level 1, 2 and 3 - Github Folder

Also, use Spring Initializr with version 2.3.1. Do NOT use snapshot versions.

### Step 1 - Setting up a Spring Project using <http://start.spring.io>

Let's use <http://start.spring.io>.

Use default settings.

Open Eclipse and "Import Existing Maven Projects".

See <https://github.com/HenryFBP/Spring-Framework-Master-Class/tree/master/spring-in-5-steps> for pom.xml.

### Step 2 - Understanding Tight Coupling using the Binary Search Algorithm Example

See commit `57067849eacdd31b2fc6847eb0ebd064662e9bc2`.

### Step 3 - Making the Binary Search Algorithm Example Loosely Coupled

The solution to tight coupling is to use an Interface in Java.

See commit `b89076ee702e351a48e1f567fc552f14a6a79ad6`

### Step 4 - Using Spring Framework to Manage Dependencies - @Component, @Autowired

(A bean is an instance of a class)

Spring needs to know:

- What are the beans?
    - use `@Component`
- What are the bean's dependencies?
    - use `@Autowired` to annotate a field as a dependency
- Where can I search for beans?

See commit `850ce6e8eaf825d0ac0aba552b84b6178a6c4f84`

### Step 5 - What is happening in the background?

Set this prop:

`logging.level.org.springframework = debug`

in the `application.properties` file.

Then search stdout for "QuickSortAlgorithm".

See commit `1f9fd3fd145fade1c904b40074cd2a5c9d0b0cb4`

### Step 6 - Dynamic auto wiring and Troubleshooting - @Primary

If there are 2 components that could either be preferred as a dependency, an error will be generated.

You need to either use "@Primary" or "@Qualifier".

See `3007cfe1b4232738dcf96890e64d611a572f4fc3` - `@Primary`.

<https://www.baeldung.com/spring-qualifier-annotation> - `@Qualifier`.

### Fastest Approach to Solve All Your Exceptions

<https://github.com/in28minutes/in28minutes-initiatives/blob/master/The-in28Minutes-TroubleshootingGuide-And-FAQ/quick-start.md>

### Step 7 - Constructor and Setter Injection

Using `@Autowired` can prevent you from having to use constructors and setters.

### Step 8 - Spring Modules

Spring is actually not one single framework.

Spring has a bunch of different submodules:

    Maven: org.springframework:spring-aop:5.3.8
    Maven: org.springframework:spring-beans:5.3.8
    Maven: org.springframework:spring-context:5.3.8
    Maven: org.springframework:spring-core:5.3.8
    Maven: org.springframework:spring-expression:5.3.8
    Maven: org.springframework:spring-jcl:5.3.8
    Maven: org.springframework:spring-test:5.3.8

We've just been using:

- Beans
- Core
- Context

Nothing else. No DB, no web, no mesaging or test tools.

Spring actually has great integration with data layers, JDBC, ORM (Hibernate/myBatis), JMS, object-to-xml, etc.

### Step 9 - Spring Projects

- Boot
- Batch
    - Batch app support
- Security
    - Securing your apps
- Cloud
    - Cloud native apps
- HATEOAS
    - Develop HATEOAS compatible services (REST but with links and discoverability)
- Data
    - Consistent data access
- Integration
    - Addresses problems with application integration.
- etc

### Step 10 - Why is Spring Popular?

1.  Enables writing testable code...
    1.  Dependency Injection makes it easy to write unit tests
    2.  Has good integration with JUnit and Mockito
2.  No plumbing code.
    1.  This means you don't need to set up database pipes, query try-catch, resource handling...this is all done for you by Spring
3.  Spring is flexible and modular.
    1.  We can use just MVC, just Boot, just DB, etc.
    2.  It also works well with other frameworks.
4.  Staying current
    1.  Spring comes up with new features that keep it relevant, like Spring Cloud, Boot.

## Section 4: Spring Level 2 - Spring Framework in Depth

### Section Introduction - Spring Framework in Depth

...

### Step 11 - Dependency Injection - A few more examples

You can also use DI to hook up DAOs and services as well.

### Step 12 - Autowiring in Depth - by Name and @Primary

If your bean resolution is ambiguous to Spring, it will error:

```
Field sortAlgorithm in com.henry.spring.basics.springin5steps.BinarySearchImpl required a single bean, but 2 were found:
	- bubbleSortAlgorithm: defined in file [C:\Users\henry\Github\Spring-Framework-Master-Class\spring-in-5-steps-level-2\target\classes\com\henry\spring\basics\springin5steps\BubbleSortAlgorithm.class]
	- quickSortAlgorithm: defined in file [C:\Users\henry\Github\Spring-Framework-Master-Class\spring-in-5-steps-level-2\target\classes\com\henry\spring\basics\springin5steps\QuickSortAlgorithm.class]

Action:

Consider marking one of the beans as @Primary, updating the consumer to accept multiple beans, or using @Qualifier to identify the bean that should be consumed
```

See commit `57bdb3af04534ae89b5e7596338c78adf582ecdd`.

Use `@Primary` to solve this if depending on an interface with multiple implementations, in commit `e318550207d6456805f2c330140a1ea196cda2b9`.

Or, like in this commit, `48bc0d30cb1b083694745cfb1c6cf37400be339b`, autowire by name:

`private SortAlgorithm bubbleSortAlgorithm;`

Note that `@Primary` has higher priority over name autowiring.

### Step 13 - Autowiring in Depth - @Qualifier annotation

Alternatively, you can use `@Qualifier("foobar")` to name beans and prefer them.

See commit `a876f5e1c599f987266ae5e5e09e85042e0a7fbc`.

### Step 14 - Scope of a Bean - Prototype and Singleton

(Remember "bean" means "instance of a class"...)

Default bean scope is Singleton.

- singleton - One instance per Spring Context
- prototype - New bean whenever requested
- request - One bean per HTTP request
- session - One bean per HTTP session

Use `@Scope()` annotation. See `353de2658dce513de2ea2459244f6e7cc1705afd`.

### Step 15 - Complex Scope Scenarios of a Spring Bean - Mix Prototype and Singleton

What happens if PersonDAO is a singleton, but its dependency, JDBCConnection, is 'prototype' scoped?

Meaning, we want ONE PersonDAO, but every PersonDAO (1) should have a unique JDBCConnection.

```java
PersonDAO personDAO = applicationContext.getBean(PersonDAO.class);
PersonDAO personDAO2 = applicationContext.getBean(PersonDAO.class);

LOGGER.info("{}", personDAO);
LOGGER.info("{}", personDAO.getJdbcConnection());
LOGGER.info("{}", personDAO2);
LOGGER.info("{}", personDAO2.getJdbcConnection());
```

Results in:

```
2021-07-30 16:45:29.008  INFO 12404 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.PersonDAO@388ffbc2
2021-07-30 16:45:29.010  INFO 12404 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.JdbcConnection@2187fff7
2021-07-30 16:45:29.010  INFO 12404 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.PersonDAO@388ffbc2
2021-07-30 16:45:29.010  INFO 12404 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.JdbcConnection@2187fff7
```

We only get 1 JdbcConnection object. This is because we're asking for a new "PersonDAO" and just get the same one, and that 1 PersonDAO has the same JdbcConnection object.

If we really want a new instance on JdbcConnection, we need to configure a "Proxy"...

Instead of giving PersonDAO a JdbcConnection object, we give it a proxy that makes sure it gets a new instance of a JdbcConnection object each time.

We do this thusly:

```java
@Component
@Scope(value = SCOPE_PROTOTYPE, proxyMode = TARGET_CLASS) //proxymode=target_class means make sure there's a new instance of this class, even with singleton parents that require me
public class JdbcConnection {
    public JdbcConnection() {
        System.out.println("JDBC Connection is coolio! ;)");
    }
}
```

Now our logs look like this:

```
2021-07-30 16:50:12.284  INFO 13680 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.PersonDAO@52350abb
JDBC Connection is coolio! ;)
2021-07-30 16:50:12.286  INFO 13680 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.JdbcConnection@486be205
2021-07-30 16:50:12.297  INFO 13680 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.PersonDAO@52350abb
JDBC Connection is coolio! ;)
2021-07-30 16:50:12.297  INFO 13680 --- [           main] c.h.s.b.s.SpringIn5StepsScopeApplication : com.henry.spring.basics.springin5steps.scope.JdbcConnection@f713686

```
We can see the JdbcConnection objects both have new hashes - 486be205 and f713686.

See commit `afb4c1e897bb91ad022d6d318f4bf7261f6c4e85`

### Step 15B - Difference Between Spring Singleton and GOF Singleton

```java
        LOGGER.info("{}", personDAO);
        LOGGER.info("{}", personDAO.getJdbcConnection());
        LOGGER.info("{}", personDAO.getJdbcConnection());

        LOGGER.info("{}", personDAO2);
        LOGGER.info("{}", personDAO2.getJdbcConnection());
```

On line 2 and 3, we get a NEW JdbcConnection object because of annotating the `PersonDAO` as such:

```java

@Component
@Scope(value = SCOPE_PROTOTYPE, proxyMode = TARGET_CLASS) //proxymode=target_class means make sure there's a new instance of this class, even with singleton parents that require me
public class JdbcConnection {
    public JdbcConnection() {
        System.out.println("JDBC Connection is coolio! ;)");
    }
}
```

Interestingly, when a JdbcConnection class is autowired like below:

```java
@Component
@Scope(SCOPE_SINGLETON)
public class PersonDAO {

    @Autowired
    JdbcConnection jdbcConnection;

...
```

A proxy gets autowired into the actual instance:

```java
result = {JdbcConnection$$EnhancerBySpringCGLIB$$de69c091@3590} "com.henry.spring.basics.springin5steps.scope.JdbcConnection@5488b5c5"
 CGLIB$BOUND = false
 CGLIB$CALLBACK_0 = {CglibAopProxy$DynamicAdvisedInterceptor@3601} 
 CGLIB$CALLBACK_1 = {CglibAopProxy$DynamicUnadvisedInterceptor@3602} 
 CGLIB$CALLBACK_2 = {CglibAopProxy$SerializableNoOp@3603} 
 CGLIB$CALLBACK_3 = {CglibAopProxy$SerializableNoOp@3604} 
 CGLIB$CALLBACK_4 = {CglibAopProxy$AdvisedDispatcher@3605} 
 CGLIB$CALLBACK_5 = {CglibAopProxy$EqualsInterceptor@3606} 
 CGLIB$CALLBACK_6 = {CglibAopProxy$HashCodeInterceptor@3607} 
```

When you call `getJdbcConnection`, that's when you get the real JdbcConnection object.

...

Onto singletons vs "gang of 4" design pattern.

Bean Scope:

Default - singleton
- singleton: 1 instance per Spring Context (NOT per JVM! There can be multiple app contexts!)
- prototype - New bean when requested (e.g. `JdbcConnection`)
- request - 1 bean per HTTP request
- session - 1 bean per HTTP session

When we talk about spring singletons, there can be 1 instance per 1 app context.

If there are 5 contexts running in the same JVM, then you *can* have 5 instances of singletons.

However, if you have a singleton defined as "Gang of Four" design pattern, it'll be 1 instance.

This difference is relevant when talking about app context, bean factory, and IOC.

### Step 16 - Using Component Scan to scan for beans

`@ComponentScan` annotation allows a class to look OUTSIDE of its top-level package for components. Can be used as follows:

```java
@SpringBootApplication
@ComponentScan("com.henry.spring.basics.outsidepackagename")
public class SomeClassThatNeedsOutsidePackages {
    ...
}
```

See commit `f5d7774d2b06ed3d8db93c7e8d59a5277207949d`.

### Step 17 - Lifecycle of a Bean - @PostConstruct and @PreDestroy

Beans are entirely managed by Spring IOC Container, if they are annotated with `@Component`.

But what if we want to do something specfic during the creating of a bean? Or after? Or before it gets destroyed?

You use the `@PostConstruct` annotation.

Example:

```java


@Component
@Scope(SCOPE_PROTOTYPE) //give a new bean whenever requested...
public class BinarySearchImpl {

    ...

    @PostConstruct
    public void postConstruct() {
        logger.info("postConstruct method called");
    }

    ...

}
```

You can also use `@PreDestroy`.

See commit `28c9cfc93cad001205cbbec5aecb6daeb85f781e`.

### Step 18 - Container and Dependency Injection (CDI) - @Named, @Inject

CDI is Context and Dependency Injection.

- CDI tries to standardize Spring Boot's concepts as part of the Java EE DI standard (JSR-330)
- Spring supports most annotations
    - @Inject (@Autowired)
    - @Named (@Component & @Qualifier)
    - @Singleton (defines scope of singleton)

CDI is more of an interface. And Spring provides the implementation.

Just like how JPA is an interface, and Hibernate is an implementation.

This means you can use "@Inject" in place of "@Autowired". Same for "@Named" in place of "@Component".

In fact, `javax.inject` dep provides these:

    Inject
    Named
    Provider
    Qualifier
    Scope
    Singleton

So, should we use CDI or Spring?

It's up to you. Because Spring is the implementation anyway, so it doesn't really matter.

See commit `c2cb7b1f278719838ca70122c6b62e4798e0318d`.

### Ignore SLF4J Errors in Step 19 - We will fix them in Step 20

### Step 19 - Removing Spring Boot in Basic Application

This is an example to show you how to use Spring without Spring Boot.

See commit `39290cb132d407b82bfe66eb3fc5ae83d819289d`, or code below.

```java
@Configuration
@ComponentScan("com.henry.spring.basics.springin5steps.basic")
public class SpringIn5StepsBasicApplication {

    public static void main(String[] args) {

        System.out.println("hello world!");
        
        AnnotationConfigApplicationContext acac =
                new AnnotationConfigApplicationContext(SpringIn5StepsBasicApplication.class);

        //get BinarySearchImpl from app context
...
```

### Step 20 - Fixing minor stuff - Add Logback and Close Application Context

See commits near `39290cb132d407b82bfe66eb3fc5ae83d819289d`.

### Step 21 - Defining Spring Application Context using XML - Part 1

Before Spring 2.5, annotations were not used. XML files were used to annotate classes and instance variables.

See <https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans> for example of `applicationContext.xml` file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

See our work in `7df20b3921fc8ec8b18e542dc74539cb7ae69a4a`.

### Step 22 - Defining Spring Application Context using XML - Part 2

Fixed my error in `29641f5f68bd041b32fcfd948e4d9f4fdf7fc399`. See edit to XML file.

### Step 23 - Mixing XML Context with Component Scan for Beans defined with Annotations

See commit `62ac932e796f86e354381e2b49fed50d0139bfe6`.

### Step 24 - IOC Container vs Application Context vs Bean Factory

Disambiguation:

- IOC Container
    - IOC is control moving out of a controller (needs a dep) and into a framework that handles it
    - Spring is an IOC Container
    - The program/framework that facilitates IOC is called an IOC Container
- App Context
    - A way to instantiate Beans
    - It is aware of classes you've defined
    - i18n capabilities
    - "Bean Factory++"
- Bean Factory
    - Simple way to create classes

### Step 25 - @Component vs @Service vs @Repository vs @Controller

- @Component - generic component
- @Repository - encapsulates storage, retrieval, and search, typically from a relational db 
- @Service - Business service facade
- @Controller - controller in MVC pattern

See commit `a9de6fd9ac848c4615149dd2bd3b269567d2ab52`.

### Step 26 - Read values from external properties file

See commit `408e011d858156b4f1b5e1319eec1b6d10ab91e1`

## Section 5: Basic Tools and Frameworks - JUnit in 5 Steps

### Section Introduction - JUnit in 5 Steps

See <https://github.com/in28minutes/spring-master-class/tree/master/00-framework-tool-introductions>

### Step 1 : What is JUnit and Unit Testing?

Skipped

### Step 2 : First JUnit Project and Green Bar

See commit `f29b32ef2bb5863b87f3aedffaf7845b6b1cfd9c`.

### Step 3 : First Code and First Unit Test

Skipped

### Step 4 : Other assert methods

Skipped

### Step 5 : Important annotations

Skipped

## Section 6: Basic Tools and Frameworks - Mockito in 5 Steps

### Section Introduction - Mockito in 5 Steps

...

### Step 1 : Setting up an example using http://start.spring.io

See commit `9401a52a8a7724b405ed50b6b9b7975f14029602` 

### COURSE UPDATE : JUnit 4 vs JUnit 5

There are small differences b/w JUnit4/JUnit5 annotations. 

But they both still work, you just need to change annotations.

### Step 2 : Using a Stubs - Disadvantages

1. You have to maintain the stubs
2. You have to keep making different versions of the stub

This is where mocks come in. Mocks make it easy to dynamically create classes.

See stub example in commit `85b2ff32134ff4d3dbeb60b049e9242ebdfa9274`.

### Step 3 : Your first mock with Mockito

See commit `c8aa5119c2bd0ee681ed31592f3e8ff505cd31cf`.

### Step 4 : Using Mockito Annotations - @Mock, @InjectMocks, @RunWith

See commit `eed25169f59b9a61f86353c636abbb14bb2e2894`.

### Step 5 : Mocking List interface

See commit `4d77bf62e543d1f96e494f48ae6f701a5f7720a8`

## Section 7: Spring Level 3 - Unit Testing with Spring Framework

### Section Introduction - Unit Testing with Spring Framework

### Step 27 - Spring Unit Testing with a Java Context

See `a97eec299baa1be1c8d45984514aaf989d9eb3e9`.

### Spring Unit Testing with an XML Context

See `2387a7ec2ef13138f99a7ce9fa47e6a5e1831a64`.

### Spring Unit Testing with Mockito

See `0f6fc4e8d6b609d33c646c668d20cb2d1da53642`.

Avoid using Spring in unit tests. Use Mockito instead.

## Section 8: Spring Level 4 - Spring Boot in 10 Steps

### Section Introduction - Spring Boot in 10 Steps

...

### Step 1 : Introduction to Spring Boot - Goals and Important Features

Goals:
-   Enable prod-ready apps fast
-   Provide common non-functional features
    -   Embedded servers
    -   Metrics
    -   Health checks
    -   Externalized config

Spring boot is NOT:
-   zero code generation
-   an app/web server

Features:
-   Quickly make spring apps
-   embedded servers - tomcat, jetty, or undertow
-   Quick start projects with auto config
    -   Web
    -   JPA (Hibernate)
-   Prod-ready features
    -   Metrics and health checks
    -   Externalized config

### Step 2 : Developing Spring Applications before Spring Boot

Spring Boot used to not exist.

This is an old project developed without Spring Boot: <https://github.com/in28minutes/SpringMvcStepByStep/blob/master/Step37.md>

- Look how large the pom.xml is!!! <https://github.com/in28minutes/SpringMvcStepByStep/blob/master/Step37.md#pomxml>
- We had to manually add a shitload of deps! And decide their versions!!
- We had to implement default exception handling! <https://github.com/in28minutes/SpringMvcStepByStep/blob/master/Step37.md#srcmainjavacomin28minutescommonexceptioncontrollerjava>
    - Why?!
- Big ass config file <https://github.com/in28minutes/SpringMvcStepByStep/blob/master/Step37.md#srcmainwebappweb-inftodo-servletxml>
- All sorts of URL filters in <https://github.com/in28minutes/SpringMvcStepByStep/blob/master/Step37.md#srcmainwebappweb-infwebxml>
- Custom logging/i8n config as well

Spring Boot automatically provides all of those settings for you.

### Step 3 : Using Spring Initializr to create a Spring Boot Application

See `11c795319c451619c4c449b461d8887e1bf5ecb3`

### Step 4 : Creating a Simple REST Controller

//localhost:8080/books -> [book1, book2, ... ]

`9efa97ccd3eae8063b61547d6a9502dfa30a9625`.

### Step 5 : What is Spring Boot Auto Configuration?

How did all the things needed for a REST service to run get auto-config'd?

The annotation here:

```java
@SpringBootApplication //<--- HERE
public class SpringbootIn10StepsApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringbootIn10StepsApplication.class, args);
	}

}
```

1. It indicates that this is a Spring Context.
2. It enables "Auto Configuration".
3. It enables "Component Scan". It scans an entire package for beans.

Then, in our BookController.java:
```java
@RestController //<-- HERE
public class BookController {

    @GetMapping("/books")
    public List<Book> getAllBooks(){
        return Arrays.asList(
                new Book(1,"Mastering Spring", "Ranga Karanam")
        );
        //gee, wonder how this returns JSON when queried...magic? or spring?
    }

}
```

We use `@RestController`. This causes `BookController` to be registered as a bean.

...

spring-boot-autoconfigure has a lot of logic built into it.

If you go into your project and open:

    org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration

These classes are responsible for creating these beans.

Look at "Example Auto Configuration" at <https://www.springboottutorial.com/spring-boot-auto-configuration>

We can also make `application.properties` and enable debug logging.

```java
logging.level.org.springframework=DEBUG
```

See `7738603e669a77b95a98f44096f41474b1a89a99`.

### Step 6 : Spring Boot vs Spring vs Spring MVC

From <https://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring>.

- Spring Framework
- Spring MVC
- Spring Boot

These frameworks have their own roles. They're not really competing. They solve different problems, and solve them well.

#### Spring Framework

Spring solves testability. It enables apps to be loosely coupled when they are developed.

IOC is at the heart of Spring. Dependency Injection is how this is achieved.

1. Define a bean
2. Define dependencies
3. Set up dependency resolution

It also prevents you from having to write a bunch of boilerplate code.

It also provides good integration with Hibernate, MyBatis, JUnit and Mockito.

#### Spring MVC

Spring MVC provides a decoupled way of developing web applications.

It allows you to separate all the different types of functionality into separate places.

#### Spring Boot

Allows you to provide automatic config.

Also, gives you starter projects built around good patterns.

Also provides good monitoring features.

### Step 7 : Spring Boot Starter Projects - Starter Web and Starter JPA

This is the pom.xml from our "Book" app's `spring-boot-starter-web` dependency:

Note that we have json and tomcat dependency which was why we were able to serve JSON responses over HTTP.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd" xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <!-- This module was also published with a richer model, Gradle metadata,  -->
  <!-- which should be used instead. Do not delete the following line which  -->
  <!-- is to indicate to Gradle or any Gradle module metadata file consumer  -->
  <!-- that they should prefer consuming it instead. -->
  <!-- do_not_remove: published-with-gradle-metadata -->
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <version>2.5.3</version>
  <name>spring-boot-starter-web</name>
  <description>Starter for building web, including RESTful, applications using Spring MVC. Uses Tomcat as the default embedded container</description>
  <url>https://spring.io/projects/spring-boot</url>
  <organization>
    <name>Pivotal Software, Inc.</name>
    <url>https://spring.io</url>
  </organization>
  <licenses>
    <license>
      <name>Apache License, Version 2.0</name>
      <url>https://www.apache.org/licenses/LICENSE-2.0</url>
    </license>
  </licenses>
  <developers>
    <developer>
      <name>Pivotal</name>
      <email>info@pivotal.io</email>
      <organization>Pivotal Software, Inc.</organization>
      <organizationUrl>https://www.spring.io</organizationUrl>
    </developer>
  </developers>
  <scm>
    <connection>scm:git:git://github.com/spring-projects/spring-boot.git</connection>
    <developerConnection>scm:git:ssh://git@github.com/spring-projects/spring-boot.git</developerConnection>
    <url>https://github.com/spring-projects/spring-boot</url>
  </scm>
  <issueManagement>
    <system>GitHub</system>
    <url>https://github.com/spring-projects/spring-boot/issues</url>
  </issueManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter</artifactId>
      <version>2.5.3</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-json</artifactId>
      <version>2.5.3</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-tomcat</artifactId>
      <version>2.5.3</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.3.9</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.9</version>
      <scope>compile</scope>
    </dependency>
  </dependencies>
</project>

```

Each of those subdeps have hibernate, validation API, jackson (json lib), logging, etc.

The point is, Spring Boot Starter projects provide a wide array of useful libraries.

### Step 8 : Overview of different Spring Boot Starter Projects

From <https://www.springboottutorial.com/spring-boot-vs-spring-mvc-vs-spring>

- spring-boot-starter-web-services - SOAP Web Services
    - Like REST, but different slightly. Not mutex to eachother.
- spring-boot-starter-web - Web & RESTful applications
- spring-boot-starter-test - Unit testing and Integration Testing
- spring-boot-starter-jdbc - Traditional JDBC
- spring-boot-starter-hateoas - Add HATEOAS features to your services
- spring-boot-starter-security - Authentication and Authorization using Spring Security
- spring-boot-starter-data-jpa - Spring Data JPA with Hibernate
- spring-boot-starter-cache - Enabling Spring Framework’s caching support
- spring-boot-starter-data-rest - Expose Simple REST Services using Spring Data REST
    - Makes it easy to expose Spring Boot Data JPA Entities as RESTful web services.

There are a few starters for technical stuff as well

- spring-boot-starter-actuator - To use advanced features like monitoring & tracing to your application out of the box
- spring-boot-starter-undertow, spring-boot-starter-jetty, spring-boot-starter-tomcat - To pick your specific choice of Embedded Servlet Container
- spring-boot-starter-logging - For Logging using logback
- spring-boot-starter-log4j2 - Logging using Log4j2

Spring Boot aims to enable production ready applications in quick time.

- Actuator : Enables Advanced Monitoring and Tracing of applications.
- Embedded Server Integrations - Since server is integrated into the application, I would NOT need to have a separate application server installed on the server.
- Default Error Handling

### Step 9 : Spring Boot Actuator

Add these to pom.xml:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.data</groupId>
    <artifactId>spring-data-rest-hal-explorer</artifactId>
</dependency>
```

Actuator exposes a lot of REST services. They are compliant with "HAL" standard.

We use a "HAL" browser to view the data. Ex: <http://api.opensupporter.org/hb2/browser.html#/api/v1>

After adding those 2 dependencies to pom.xml, visit <http://localhost:8080/actuator>

Note it's kind of empty.

Add `management.endpoints.web.exposure.include=*` to your .properties file. Then reload and note how many new things are showing up. Note this is not good practice to enable all endpoints.

Go to <http://localhost:8080/explorer/index.html#uri=http://localhost:8080/actuator> and see that you can use this built in HAL browser to explore the API.

See commit `02c3d29c101d3acbd4c83033b5ab64bf2e055260`.

### Step 10 : Spring Boot Developer Tools

If we edit java source code, we need to restart the entire REST server for the changes to take effect. PITA, right?

This is where this dep helps:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
</dependency>
```

See commit `830ca3f67df7f9ef1ffb377abfbc01f3ac588f24`.

## Section 9: Spring Level 5 - Spring AOP

### Section Introduction - Spring AOP

What TF is AOP?

- Pointcut
- Advice
- Aspect

- JoinPoint
- Weaving
- Weaver

- @Before, @Around, etc

- Types of Pointcut definitions

- Custom annotation to track time

### Spring AOP Github Folder

<https://github.com/in28minutes/spring-master-class/tree/master/03-spring-aop>

### COURSE UPDATE - AOP Dependency Removed From Spring Initializr

In the next step, we use Spring AOP. However, AOP dependency is not available on the Spring Initializr website anymore.

Recommendation:

STEP I: Create a project as shown in the next step without adding AOP.

STEP II: Add this dependency into your pom.xml.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### Step 01 - Setting up AOP Example - Part 1

Sometimes, in "layered" apps, we have "cross-cutting concerns" like logging and security, that go across multiple "layers" of the app (across database, business, presentation layer).

These are not related to specific layers. Perfmon/logging/security can apply to all layers.

Aspect Oriented Programming.

We have 2 DAOs and 2 Business services.

We want to intercept calls to these classes. See that in next step.

See commit `0474111e42976b71253336bc1c9ddf603dd352ee`

### Step 02 - Setting up AOP Example - Part 2

See commit `c6869e3ee3426473cdfc3a3541524fad2a290d2e`.

### Step 03 - Defining an @Before advice

See this code.

```java
package com.henry.spring.aop.springaop.aspect;


import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.annotation.Configuration;

//config and related to AOP
@Configuration
@Aspect
public class BeforeAspect {

    private Logger logger = LoggerFactory.getLogger(this.getClass());

    //What methods do I intercept?
    // execution(* PACKAGE.*.*(..))
    //           ^         ^ ^  ^
    //           |         | |  \----any arguments
    //           |         | |
    //           |         | \--- all method calls
    //           |         |
    //           |         any class
    //           |
    //           any return type

    @Before("execution(* com.henry.spring.aop.springaop.business.*.*(..)") //pass a pointcut to before annotation
    public void before(){
        //what do I do?
        logger.info("intercepted method calls");
    }
}

```

See commit `16df3012328d30bbd3aa3facaba002d593d3de46`.

### Step 04 - Understand AOP Terminology - Pointcut, Advice, Aspect and Join Point

### Step 05 - Using @After, @AfterReturning, @AfterThrowing advices

### Step 06 - Using @Around advice to implement performance tracing

### Step 07 - Best Practice : Use common Pointcut Configuration

### Step 08 - Quick summary of other Pointcuts

### Step 09 - Creating Custom Annotation and an Aspect for Tracking Time

## Section 10: Spring Level 6 - Interacting with Databases - Spring JDBC, JPA and Spring Data

### Section Introduction - Spring JDBC, JPA and Spring Data

### Spring JDBC to JPA with Hibernate - Github Folder

### Step 01 - Setting up a project with JDBC, JPA, H2 and Web Dependencies

### COURSE UPDATE : H2 Database URL

### Step 02 - Launching up H2 Console

### Updates to Step 03 and Step 04

### Step 03 - Creating a Database Table in H2

### Step 04 - Populate data into Person Table

### Step 05 - Implement findAll persons Spring JDBC Query Method

### Step 06 - Execute the findAll method using CommandLineRunner

### Step 07 - A Quick Review - JDBC vs Spring JDBC

### Step 08 - Whats in the background? Understanding Spring Boot Autoconfiguration

### Step 09 - Implementing findById Spring JDBC Query Method

### Step 10 - Implementing deleteById Spring JDBC Update Method

### Step 11 - Implementing insert and update Spring JDBC Update Methods

### Step 12 - Creating a custom Spring JDBC RowMapper

### Step 13 - Quick introduction to JPA

### Step 14 - Defining Person Entity

### Step 15 - Implementing findById JPA Repository Method

### Step 16 - Implementing insert and update JPA Repository Methods

### Step 17 - Implementing deleteById JPA Repository Method

### Step 18 - Implementing findAll using JPQL Named Query

### Step 19 - Introduction to Spring Data JPA

### Step 20 - Connecting to Other Databases

## Section 11: Quick Preview - Web Applications With Spring MVC

### Section Introduction - Basic Web Application

### Links for the Next Lecture

### Step 01 : Setting up Your First Java Web Application

### Step 01 : Theory 1 - Maven and Magic

### Step 01 : Theory 2 - What is a Servlet?

### Step 01 : Theory 3 - Web Application Request Flow

### Step 01 : Theory 4 - Understand Your First Servlet - LoginServlet

### Step 02 : Create LoginServlet From Scratch Again and Your First View

### Step 02 : Theory - Play Time - Let's Try Breaking Things

### Step 03 : Passing Request Parameters using Get Method

### Step 03 : Theory - Introduction and End to Scriptlets

### Step 04 : Disadvantages of Get Parameters

### Step 05 : Your First Post Request

### Step 06 : Your First Servlet doPost Method

### Step 07 : Lets Add a Password Field

### Step 10 : Setting up Maven, Tomcat and Simple JEE Application

### Links for the Next Lecture

### Step 11 : Setting up Spring MVC with 4 mini steps

### Step 12 : Your First Spring MVC Controller

### Step 13 : Part 1 - Your First Spring MVC View : ViewResolver

### Step 13 : Part 2 - Theory Break - Spring MVC Architecture

### Step 13 : Part 3 - Play Break - Try Breaking Things

### Step 14 : Add Logging Framework Log4j

### Step 15 : Redirect to Welcome Page: ModelMap and @RequestParam

### Step 16 : Use LoginService to Authenticate

### Step 17 : Spring Autowiring and Dependency Injection

## Section 12: Basic Tools and Frameworks - Eclipse in 5 Steps

### Section Introduction - Eclipse in 5 Steps

### Step 1 : Create a Java Project

### Step 2 : Keyboard Shortcuts

### Step 3 : Views and Perspectives

### Step 4 : Save Actions

### Step 5 : Code Generation

## Section 13: Basic Tools and Frameworks - Maven in 5 Steps

### Section Introduction - Maven in 5 Steps

### Step 1 : Creating and importing a Maven Project

### Step 2 : Understanding Project Object Model - pom.xml

### Step 3 : Maven Build Life Cycle

### Step 4 : How does Maven Work?

### Step 5 : Important Maven Commands

## Section 14: Congratulations

### Bonus Lecture

### Spring Master Class - Congratulations on Completing the Course

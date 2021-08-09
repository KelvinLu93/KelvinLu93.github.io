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



### Step 18 - Container and Dependency Injection (CDI) - @Named, @Inject

### Ignore SLF4J Errors in Step 19 - We will fix them in Step 20

### Step 19 - Removing Spring Boot in Basic Application

### Step 20 - Fixing minor stuff - Add Logback and Close Application Context

### Step 21 - Defining Spring Application Context using XML - Part 1

### Step 22 - Defining Spring Application Context using XML - Part 2

### Step 23 - Mixing XML Context with Component Scan for Beans defined with Annotati

### Step 24 - IOC Container vs Application Context vs Bean Factory

### Step 25 - @Component vs @Service vs @Repository vs @Controller

### Step 26 - Read values from external properties file

## Section 5: Basic Tools and Frameworks - JUnit in 5 Steps

35. Section Introduction - JUnit in 5 Steps

36. Step 1 : What is JUnit and Unit
Testing?

37. Step 2 : First JUnit Project and Green
Bar

38. Step 3 : First Code and First Unit Test

39. Step 4 : Other assert methods

40. Step 5 : Important annotations

## Section 6: Basic Tools and Frameworks - Mockito in 5 Steps

41. Section Introduction - Mockito in 5 Steps

42. Step 1 : Setting up an example using http://start.spring.io

43. COURSE UPDATE : JUnit 4 vs JUnit 5

44. Step 2 : Using a Stubs - Disadvantages

45. Step 3 : Your first mock with Mockito

46. Step 4 : Using Mockito Annotations - @Mock, @InjectMocks, @RunWith

47. Step 5 : Mocking List interface

## Section 7: Spring Level 3 - Unit Testing with Spring Framework

48. Section Introduction - Unit Testing with Spring Framework

49. Step 27 - Spring Unit Testing with a Java Context

50. Spring Unit Testing with an XML Context

51. Spring Unit Testing with Mockito

## Section 8: Spring Level 4 - Spring Boot in 10 Steps

52. Section Introduction - Spring Boot in 10 Steps

53. Step 1 : Introduction to Spring Boot - Goals and Important Features

54. Step 2 : Developing Spring Applications before Spring Boot

55. Step 3 : Using Spring Initializr to create a Spring Boot Application

56. Step 4 : Creating a Simple REST Controller

57. Step 5 : What is Spring Boot Auto Configuration?

58. Step 6 : Spring Boot vs Spring vs Spring MVC

59. Step 7 : Spring Boot Starter Projects - Starter Web and Starter JPA

60. Step 8 : Overview of different Spring Boot Starter Projects

61. Step 9 : Spring Boot Actuator

62. Step 10 : Spring Boot Developer Tools

## Section 9: Spring Level 5 - Spring AOP

## Section 10: Spring Level 6 - Interacting with Databases - Spring JDBC, JPA and Spring Data

75. Section Introduction - Spring JDBC, JPA and Spring Data

76. Spring JDBC to JPA with Hibernate - Github Folder

77. Step 01 - Setting up a project with JDBC, JPA, H2 and Web Dependencies

78. COURSE UPDATE : H2 Database URL

79. Step 02 - Launching up H2 Console

80. Updates to Step 03 and Step 04

81. Step 03 - Creating a Database Table in H2

82. Step 04 - Populate data into Person
Table

83. Step 05 - Implement findAll persons Spring JDBC Query Method

84. Step 06 - Execute the findAll method using CommandLineRunner

85. Step 07 - A Quick Review - JDBC vs Spring JDBC

86. Step 08 - Whats in the background? Understanding Spring Boot Autoconfiguration

87. Step 09 - Implementing findById Spring JDBC Query Method

88. Step 10 - Implementing deleteById Spring JDBC Update Method

89. Step 11 - Implementing insert and update Spring JDBC Update Methods

90. Step 12 - Creating a custom Spring JDBC RowMapper

91. Step 13 - Quick introduction to JPA

92. Step 14 - Defining Person Entity

93. Step 15 - Implementing findById JPA Repository Method

94. Step 16 - Implementing insert and update JPA Repository Methods

95. Step 17 - Implementing deleteById JPA Repository Method

96. Step 18 - Implementing findAll using JPQL Named Query

97. Step 19 - Introduction to Spring Data JPA

98. Step 20 - Connecting to Other Databases

## Section 11: Quick Preview - Web Applications With Spring MVC

99. Section Introduction - Basic Web Application

100. Links for the Next Lecture

101. Step 01 : Setting up Your First Java Web Application

102. Step 01 : Theory 1 - Maven and Magic

103. Step 01 : Theory 2 - What is a Servlet?

104. Step 01 : Theory 3 - Web Application Request Flow

105. Step 01 : Theory 4 - Understand Your First Servlet - LoginServlet

106. Step 02 : Create LoginServlet From Scratch Again and Your First View

107. Step 02 : Theory - Play Time - Let's Try Breaking Things

108. Step 03 : Passing Request Parameters using Get Method

109. Step 03 : Theory - Introduction and End to Scriptlets

110. Step 04 : Disadvantages of Get Parameters

111. Step 05 : Your First Post Request

112. Step 06 : Your First Servlet doPost Method

113. Step 07 : Lets Add a Password Field

114. Step 10 : Setting up Maven, Tomcat and Simple JEE Application

115. Links for the Next Lecture

116. Step 11 : Setting up Spring MVC with 4 mini steps

117. Step 12 : Your First Spring MVC Controller

118. Step 13 : Part 1 - Your First Spring MVC View : ViewResolver

119. Step 13 : Part 2 - Theory Break - Spring MVC Architecture

120. Step 13 : Part 3 - Play Break - Try Breaking Things

121. Step 14 : Add Logging Framework Log4j

122. Step 15 : Redirect to Welcome Page: ModelMap and @RequestParam

123. Step 16 : Use LoginService to Authenticate

124. Step 17 : Spring Autowiring and Dependency Injection

## Section 12: Basic Tools and Frameworks - Eclipse in 5 Steps

125. Section Introduction - Eclipse in 5 Steps

126. Step 1 : Create a Java Project

127. Step 2 : Keyboard Shortcuts

128. Step 3 : Views and Perspectives

129. Step 4 : Save Actions

130. Step 5 : Code Generation

## Section 13: Basic Tools and Frameworks - Maven in 5 Steps

131. Section Introduction - Maven in 5 Steps

132. Step 1 : Creating and importing a Maven Project

133. Step 2 : Understanding Project Object Model - pom.xml

134. Step 3 : Maven Build Life Cycle

135. Step 4 : How does Maven Work?

136. Step 5 : Important Maven Commands

## Section 14: Congratulations

137. Bonus Lecture

138. Spring Master Class - Congratulations on Completing the Course

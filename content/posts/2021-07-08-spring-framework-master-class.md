---
layout: post
title: "Spring Framework Master Class"
date: 2021-07-08T10:24:50-05:00
categories: [programming, spring, web, java]
draft: false
---

From <https://udemy.com/course/spring-tutorial-for-beginners/>.

Code can be found at 

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
- Inversion of Control
- IOC Container
- Application Context
    - The most important IOC Container as far as Spring is concerned is the AC.
    - The AC creates and manages all of the beans
    - This is the most important part of SF.

### What is Spring?

Spring is a DI Framework!

## Section 2: Spring Master Class - Level 1 to Level 6 - Course Overview, Github & More

### 2. Spring Framework Master Class - Overview

- <https://github.com/in28minutes/spring-master-class>
- [Course Guide PDF](/files/2021-07-08-spring-framework-master-class/SpringMasterClass-CourseGuidev0.1.pdf)

You learn the basics of SF in 6 levels. See Course Guide.

## Section 3: Spring Level 1 - Introduction to Spring Framework in 10 Steps

### 3. Section Introduction - Spring Framework in 10 Steps

See <https://github.com/in28minutes/spring-master-class/tree/master/01-spring-in-depth> for code.

Use Eclise Oxyen.

### 4. Spring Level 1, 2 and 3 - Github Folder

Also, use Spring Initializr with version 2.3.1. Do NOT use snapshot versions.

### 5. Step 1 - Setting up a Spring Project using <http://start.spring.io>

Let's use <http://start.spring.io>.

Use default settings.

Open Eclipse and "Import Existing Maven Projects".

See <https://github.com/HenryFBP/Spring-Framework-Master-Class/tree/master/spring-in-5-steps> for pom.xml.

6. Step 2 - Understanding Tight Coupling using the Binary Search Algorithm Example

7. Step 3 - Making the Binary Search Algorithm Example Loosely Coupled

8. Step 4 - Using Spring Framework to Manage Dependencies - @Component, @Autowired

9. Step 5 - What is happening in the background?

10. Step 6 - Dynamic auto wiring and Troubleshooting - @Primary

11. Fastest Approach to Solve All Your Exceptions

12. Step 7 - Constructor and Setter Injection

13. Step 8 - Spring Modules

14. Step 9 - Spring Projects

15. Step 10 - Why is Spring Popular?

## Section 4: Spring Level 2 - Spring Framework in Depth

16. Section Introduction - Spring Framework in Depth

17. Step 11 - Dependency Injection - A few more examples

18. Step 12 - Autowiring in Depth - by Name and @Primary

19. Step 13 - Autowiring in Depth - @Qualifier annotation

20. Step 14 - Scope of a Bean - Prototype and Singleton

21. Step 15 - Complex Scope Scenarios of a Spring Bean - Mix Prototype and Singleton

22. Step 15B - Difference Between Spring Singleton and GOF Singleton

23. Step 16 - Using Component Scan to scan for beans

24. Step 17 - Lifecycle of a Bean - @PostConstruct and @PreDestroy

25. Step 18 - Container and Dependency Injection (CDI) - @Named, @Inject

26. Ignore SLF4J Errors in Step 19 - We will fix them in Step 20

27. Step 19 - Removing Spring Boot in Basic Application

28. Step 20 - Fixing minor stuff - Add Logback and Close Application Context

29. Step 21 - Defining Spring Application Context using XML - Part 1

30. Step 22 - Defining Spring Application Context using XML - Part 2

31. Step 23 - Mixing XML Context with Component Scan for Beans defined with Annotati

32. Step 24 - IOC Container vs Application Context vs Bean Factory

33. Step 25 - @Component vs @Service vs @Repository vs @Controller

34. Step 26 - Read values from external properties file

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
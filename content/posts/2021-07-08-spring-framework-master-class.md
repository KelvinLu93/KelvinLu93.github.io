---
layout: post
title: "Spring Framework Master Class"
date: 2021-07-08T10:24:50-05:00
categories: [programming, spring, web, java]
draft: false
---

From <https://udemy.com/course/spring-tutorial-for-beginners/>.

## 1. Intro to Spring

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
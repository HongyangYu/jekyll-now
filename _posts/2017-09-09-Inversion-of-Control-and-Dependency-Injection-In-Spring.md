---
layout: post
title: Inversion of Control and Dependency Injection In Spring
---

Recently, I am developing Java API in Spring Boot Framework and learning this framework. I found a new feature from Spring Framework 4.3 that if a bean has only one constructor, @Autowired can be omitted. Then I planned to write a blog about Inversion of Control (IoC) and Dependency Injection (DI) in Spring. This article will only introduce basic definition of IoC and DI and how to use it, and I will write another blog to talk about the theory and implement of DI later. If you are just interested in the new feature, please jump to the last part.<br>

## 1. What is IoC and DI

Wikipedia: 

* IoC:
> In software engineering, inversion of control (IoC) is a design principle in which custom-written portions of a computer program receive the flow of control from a generic framework. A software architecture with this design inverts control as compared to traditional procedural programming: in traditional programming, the custom code that expresses the purpose of the program calls into reusable libraries to take care of generic tasks, but with inversion of control, it is the framework that calls into the custom, or task-specific, code.

IoC is a design principle to reduce decoupling. It means when a object needs another object, it should get a existing object generated by an IoC container instead of generating a new object by itself. That is the control of other objects is handled by an IoC container, while in traditional programming, the control is handled by object itself. This is the inversion part. So it called Inversion of Control. <br>

The following pictures show the differences between traditional programming and IoC programming.

![Traditional Programming](https://github.com/HongyangYu/hongyangyu.github.io/blob/master/images/ioc-1.png?raw=true)

Fig 1. Traditional Programming

![IoC Programming](https://github.com/HongyangYu/hongyangyu.github.io/blob/master/images/ioc-2.png?raw=true)

Fig 2. IoC Programming

* DI
> In software engineering, dependency injection is a technique whereby one object supplies the dependencies of another object. A dependency is an object that can be used (a service). An injection is the passing of a dependency to a dependent object (a client) that would use it. The service is made part of the client's state. Passing the service to the client, rather than allowing a client to build or find the service, is the fundamental requirement of the pattern.


DI is a design pattern implementing IoC. It means giving an object its instance variables. 


## 2. How to use DI in Spring

### 2.1 XML Configuration

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="helloWorld" class="study.spring.helloworld.HelloWorld">
        <property name="message" value="Hello World! - Hongyang"/>
    </bean>

</beans>
```

```
public class Main {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("./Beans.xml");
        HelloWorld obj = (HelloWorld) context.getBean("helloWorld");
        obj.printMessage();
    }
}
```

```
@Data //Use lombok to simplify code. It will generate default constructor, getters, setters, hashCode and equals function
public class HelloWorld {
    private String message;
    public void printMessage(){
        System.out.println("Your Message : " + message);
    }
}
```

### 2.2 Annotaion @Autowired

```
@Service
public class ActivityService {

    @Autowired
    private ActivityRepository activityRepository;

    public List<Activity> getAllActivities() {
        return activityRepository.findAll();
    }
    //...
}

```

Personally, I prefer the annotation way because the XML configuration is more cumbersome, while the annotation way is simpler and can be used whenever I can. But, of course, either way can accomplish dependency injection. The annotation is just a way to implement it. <br>

## 3. A new feature in Spring Framework 4.3

Let's talk about the new feature I mentioned at first. You can find more new features and enhancement in Spring Framework 4.3 in [here](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/new-in-4.3.html)

> It is no longer necessary to specify the @Autowired annotation if the target bean only defines one constructor.

```
@Service
public class ActivityService {

    //@Autowired can be omitted here
    private final ActivityRepository activityRepository;

    public ActivityService(ActivityRepository activityRepository) {
        this.activityRepository = activityRepository;
    }
    // ...
}
```

## Reference
1. Wikipedia: [Dependency injection](https://en.wikipedia.org/wiki/Dependency_injection)
2. Wikipedia: [Inversion of control](https://en.wikipedia.org/wiki/Inversion_of_control)
3. Spring: [New Features and Enhancements in Spring Framework 4.3](https://docs.spring.io/spring/docs/current/spring-framework-reference/html/new-in-4.3.html)

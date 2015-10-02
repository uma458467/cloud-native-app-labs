# Spring Boot

## Requirements

[Lab Requirements](https://github.com/pivotal-enablement/cloud-native-app-labs/blob/master/lab-instructions/requirements.md)

## What You Will Learn

* How to create a Spring Boot Project

## Exercises

### Create A Spring Boot Project

1) Browse to [https://start.spring.io/](https://start.spring.io/)

![Spring Initializr](resources/images/spring-initializr.png "Spring Initializr")

2) Fill out the *Project metadata* fields as follows:

*Group*

`io.pivotal`

*Artifact*

`hello-spring-boot`

*Name*

`hello-spring-boot`

*Description*

`Hello Spring Boot`

*Package Name*

`io.pivotal.hello`

*Type*

`Maven Project`

*Packaging*

`Jar`

*Java Version*

`1.8`

*Language*

`Java`

*Spring Boot Version*

`1.2.6`

3) In the Project dependencies section, check the following:

* `Web`

3) Click the Generate Project button. Your browser will download a zip file. Unpack that zip file at

4) Import the project’s pom.xml into your editor/IDE of choice.

5) Add a @RestController annotation to the class io.pivotal.spring.hello.HelloSpringBootApplication.

6) Add the following request handler to the class io.pivotal.spring.hello.HelloSpringBootApplication:

```java
@RequestMapping("/")
public String hello() {
    return "Hello World!";
}
```

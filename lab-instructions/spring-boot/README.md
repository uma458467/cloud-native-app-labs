# Spring Boot

<!-- TOC depth:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Spring Boot](#spring-boot)
	- [Requirements](#requirements)
	- [What You Will Learn](#what-you-will-learn)
	- [Exercises](#exercises)
		- [Create A Spring Boot Project](#create-a-spring-boot-project)
<!-- /TOC -->

## Requirements

[Lab Requirements](https://github.com/pivotal-enablement/cloud-native-app-labs/blob/master/lab-instructions/requirements.md)

## What You Will Learn

* How to create a Spring Boot Project

## Exercises

### Create A Spring Boot Project

1) Browse to [https://start.spring.io/](https://start.spring.io/)

![Spring Initializr](resources/images/spring-initializr.png "Spring Initializr")

2) Fill out the *Project metadata* fields as follows:

**Group**

`io.pivotal`

**Artifact**

`hello-spring-boot`

**Name**

`hello-spring-boot`

**Description**

`Hello Spring Boot`

**Package Name**

`io.pivotal.hello`

**Type**

`Maven Project`

**Packaging**

`Jar`

**Java Version**

`1.8`

**Language**

`Java`

**Spring Boot Version**

`1.2.6`

3) In the Project dependencies section, check the following:

* `Web`

3) Click the Generate Project button. Your browser will download a zip file. Unpack that zip file into a work directory.  Recommendation: `~/repos` ($USER-HOME/repo).

4) Import the project’s pom.xml into your editor/IDE of choice.

_STS Import Help_:

Select File > Import... Then select Maven > Existing Maven Projects. On the Import Maven Projects page, browse to your `$WORK-DIRECTORY/hello-spring-boot` (e.g. ~/repos/hello-spring-boot).

5) Add a `@RestController` annotation to the class `io.pivotal.hello.HelloSpringBootApplication`.  You will need to add the import for `org.springframework.web.bind.annotation.RestController`.

_STS Shortcut Help_:

Need help adding an import?

Use the `organize imports` command:
* PC: Ctrl + Shift + O
* Mac: Cmd + Shift + O

Not sure how to resolve the problem STS is reporting?

Try the quick-fix (magic shortcut):
* PC: Ctrl + 1
* Mac: Cmd + 1

Other helpful [shortcuts](https://blog.codecentric.de/en/2012/08/my-top-10-shortcuts-for-eclipse-on-mac-os-x-and-windows-and-how-you-survive-the-change-from-windows-to-mac/).


```java
@SpringBootApplication
@RestController
public class HelloSpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(HelloSpringBootApplication.class, args);
    }
}
```
6) Add the following request handler to the class `io.pivotal.hello.HelloSpringBootApplication`.  You will need to add the import for `org.springframework.web.bind.annotation.RequestMapping;`

```java
@RequestMapping("/")
public String hello() {
    return "Hello World!";
}
```

Completed:
```java
@SpringBootApplication
@RestController
public class HelloSpringBootApplication {

    public static void main(String[] args) {
        SpringApplication.run(HelloSpringBootApplication.class, args);
    }

    @RequestMapping("/")
    public String hello() {
        return "Hello World!";
    }
}
```

7) Open a terminal window and change to `hello-spring-boot` directory:

```bash
$ cd $WORK-DIRECTORY/hello-spring-boot
```

8) Run the application
```
mvn clean spring-boot:run
```

9) You should see the application start up an embedded Apache Tomcat server on port 8080 (review terminal output):

```
2015-10-02 13:26:59.264  INFO 44749 --- [lication.main()] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)
2015-10-02 13:26:59.267  INFO 44749 --- [lication.main()] i.p.hello.HelloSpringBootApplication     : Started HelloSpringBootApplication in 2.541 seconds (JVM running for 9.141)
```

10) Browse to: [http://localhost:8080/](http://localhost:8080/)

![Hello Workd](resources/images/hello-world.png "Hello World")

**Congratulations!**  You’ve just completed your first Spring Boot application.

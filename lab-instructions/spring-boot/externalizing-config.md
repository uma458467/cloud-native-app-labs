# Externalizing Configuration with Spring Boot

## Requirements

[Lab Requirements](https://github.com/pivotal-enablement/cloud-native-app-labs/blob/master/lab-instructions/requirements.md)

## What You Will Learn

* How to externalize configuration in a Spring Boot project

## Exercises

### Refactoring to Externalize the Config

1) In the `hello-spring-boot` project rename `src/main/resources/application.properties` to `src/main/resources/application.yml`. Into that file, paste the following:

```yml
greeting: Hello
```

Spring Boot supports both configuration formats `application.properties` and `application.yml`.

2) To the class `io.pivotal.spring.hello.HelloSpringBootApplication`, add a greeting field and inject its value.  This will require importing `org.springframework.beans.factory.annotation.Value`.

```java
@Value("${greeting}")
String greeting;
```


3) Also `io.pivotal.spring.hello.HelloSpringBootApplication`, change the return statement of `hello()` to the following:

```
return String.format("%s World!", greeting);
```

Completed (Steps 2 & 3):
```java
@SpringBootApplication
@RestController
public class HelloSpringBootApplication {

	@Value("${greeting}")
	String greeting;

    public static void main(String[] args) {
        SpringApplication.run(HelloSpringBootApplication.class, args);
    }

    @RequestMapping("/")
    public String hello() {
        return String.format("%s World!", greeting);
    }
}
```

4) Run the `hello-spring-boot` application:

``` bash
$ mvn clean spring-boot:run
```

5) Visit the application in the browser (http://localhost:8080), and verify that the output is still the following:
![Hello World](resources/images/hello-world.png "Hello World")

6) Stop the `hello-spring-boot` application.

### Using Environment Variables for Config

1) Run the application again, this time setting the GREETING environment variable:

```bash
$ GREETING=Ohai java -jar target/hello-spring-boot-0.0.1-SNAPSHOT.jar
```
2) Visit the application in the browser (http://localhost:8080), and verify that the output has changed to the following:
![Ohai World](resources/images/ohai-world.png "Ohai World")

3) Stop the `hello-spring-boot` application.

### Using Spring Profiles for Config

1) Add a spanish profile to application.yml. Your finished configuration should reflect the following:

```yml
greeting: Hello

---

spring:
  profiles: spanish

greeting: Hola
```

2) Run the `hello-spring-boot` application.  This time setting the SPRING_PROFILES_ACTIVE environment variable:

```bash
$ SPRING_PROFILES_ACTIVE=spanish mvn clean spring-boot:run
```

3) Visit the application in the browser (http://localhost:8080), and verify that the output has changed to the following:

![Hola World](resources/images/hola-world.png "Hola World")

4) Stop the `hello-spring-boot` application.

### Resolving Conflicts

1) Run the `hello-spring-boot` application, this time setting both the `SPRING_PROFILES_ACTIVE` and `GREETING` environment variables:

```
$ SPRING_PROFILES_ACTIVE=spanish GREETING=Ohai mvn clean spring-boot:run
```

Visit the application in the browser (http://localhost:8080), and verify that the output has changed to the following:

![Ohai World](resources/images/ohai-world.png "Ohai World")

Visit http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html to learn more about this outcome and the entire priority scheme for conflict resolution.

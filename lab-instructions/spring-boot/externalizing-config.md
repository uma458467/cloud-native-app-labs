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

Spring Boot supports both configuration formats: traditional properties files and YAML.  YAML offers a conscise format when compared to properties files.  Additionally, support for multiple documents within one file add an added capability not present in properties files (more on this later in the lab).  For more details on externalizing configuration review the following [documentation](http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html).

2) In the class `io.pivotal.spring.hello.HelloSpringBootApplication`, add a greeting field and inject its value.  This will require importing `org.springframework.beans.factory.annotation.Value`.

```java
@Value("${greeting}")
String greeting;
```


3) Also within `io.pivotal.spring.hello.HelloSpringBootApplication`, change the return statement of `hello()` to the following:

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

5) Visit the application in the browser [http://localhost:8080](http://localhost:8080), and verify that the output is still the following:

![Hello World](resources/images/hello-world.png "Hello World")

6) Stop the `hello-spring-boot` application.

### Using Environment Variables for Config

1) Run the application again, this time setting the `GREETING` environment variable:

```bash
[mac, linux]
$ GREETING=Ohai mvn clean spring-boot:run

[windows]
$ set GREETING=Ohai
$ mvn clean spring-boot:run
```

2) Visit the application in the browser [http://localhost:8080](http://localhost:8080), and verify that the output has changed to the following:

![Ohai World](resources/images/ohai-world.png "Ohai World")

***What Just Happened?***

Instead of returning the `greeting` value from the `application.yml`, the value from the environment variable was used.  The environment variable overrides the value from the `application.yml` file.

3) Stop the `hello-spring-boot` application.

### Using Spring Profiles for Config

1) Add a spanish profile to the `application.yml`. Your finished configuration should reflect the following:

```yml
greeting: Hello

---

spring:
  profiles: spanish

greeting: Hola
```

Yaml supports having multiple documents in one file.  The first document is the default configuration.  In the second document, we  use the `spring.profiles` key to indicate when it applies.  When running with the spanish profile, use "Hola" as the greeting.


2) Run the `hello-spring-boot` application.  This time setting the `SPRING_PROFILES_ACTIVE` environment variable:

```bash
[mac, linux]
$ SPRING_PROFILES_ACTIVE=spanish mvn clean spring-boot:run

[windows]
$ set SPRING_PROFILES_ACTIVE=spanish
$ mvn clean spring-boot:run
```

3) Visit the application in the browser [http://localhost:8080](http://localhost:8080), and verify that the output has changed to the following:

![Hola World](resources/images/hola-world.png "Hola World")

***What Just Happened?***

The value for the `greeting` key was pulled from the the spanish profile yaml document, because the spanish profile is active.

4) Stop the `hello-spring-boot` application.

### Resolving Conflicts

1) Run the `hello-spring-boot` application, this time setting both the `SPRING_PROFILES_ACTIVE` and `GREETING` environment variables:

```
[mac, linux]
$ SPRING_PROFILES_ACTIVE=spanish GREETING=Ohai mvn clean spring-boot:run

[windows]
$ set SPRING_PROFILES_ACTIVE=spanish
$ set GREETING=Ohai
$ mvn clean spring-boot:run
```

Visit the application in the browser [http://localhost:8080](http://localhost:8080), and verify that the output has changed to the following:

![Ohai World](resources/images/ohai-world.png "Ohai World")

***What Just Happened?***

Instead of returning either `greeting` value from the `application.yml`, the value from the environment variable was used.  It overrides the active profile (`SPRING_PROFILES_ACTIVE`).

Visit http://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-external-config.html to learn more about this outcome and the entire priority scheme for conflict resolution.

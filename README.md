# Introduction
- Spring is a huge framework used for creating standalone Spring based applications easily.
- Spring is an application framework - that does the handling of Http requests, connections to databases ( RDBMS, MongoDB ), running queries etc.
- Sprint Boot is like a wrapper on top of it so that the setting of Spring project, working and managing its dependencies are easy.
- Basically it makes the creation of a Spring project easy - bootstrapping of Spring. Most of the configuration is offered out-of-the-box.
- This is extremely useful because most projects have some commonalities like connecting to databases, etc.
- So having this supplied by default via the Spring application using Bootstrap is what Spring Boot does.

# Questions
- Application Context
- Classpath Scan
  - Finds annotations and registers them 
- Configuration / Auto Configuration
- Spring Database connections
- Controller vs Rest Controller
- Dependency Injection
- Inversion of Control
- What to do if someone needs XML instead of JSON output?.
- Where does GraphQL fit in?.
- Integration with Cassandra
- Integration with Thymeleaf
- Integration with a sample Angular or VanillaJS application
- Integration with Redit
- SpringBoot Microservices

# Problems with Spring
1. Huge framework that can be too much for a beginner.
2. Lot of steps involved in setting up a project
3. Configuration steps
4. Multiple build and deploy steps
These steps are abstracted via Sprint Boot.

# Creating a SpringBoot project

Spring applications are typically managed by Maven. You can just create a Maven project and add the following parent/dependency.

```
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>2.1.2.RELEASE</version>
	<relativePath/>
</parent>

<dependencies>
	<dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-web</artifactId>
	</dependency>
</dependencies>
```

- The `<parent>` section inherits the configurations defined in that Maven project onto the current project and the `<dependency>` section describes the dependencies needed for the current project to be build successfully. In this case, it adds the Web related capabilities to the Spring project. **In other words, the dependencies section tells which jars etc are needed and the parent section tells which versions of those jars are going to be used.** This preset configuration is referred to as Spring **Bill of Materials**.
- The advantage of this preset is that it will determine what all combinations work well together. So as a developer you need not worry if the combinations of jars work well together or not.

The below piece of code lets us launch the bare minimum springboot application.
1. @SpringBootApplication tells Spring that this class is the SpringBoot Application.
2. Recall that SpringBoot helps in building standalone applications - hence the main method.
3. SpringApplication.run() - 
       a. Sets up default configuration.
       b. Starts Spring Application context
       c. Performs class path scan ( annotations @Service, @Controller )
       d. Starts Tomcat server - this comes with SpringBoot. 
 
```java
package io.bubblesort.springbootstarter;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CourseApiApp {

	public static void main(String[] args) {
		SpringApplication.run( CourseApiApp.class, args);
	}
}
```
- SpringApplication.run, runs the servlet container.
- Servlet Container, i.e., Tomcat, comes along with SpringBoot application to make the development easy. This makes building MicroServices with SpringBoot faster.
- The arguments on the command line are passed onto the SpringApplication static method.

# URL Mappings and Controllers

```@RequestMapping``` determines which URL is going to hit which method of the class. This mainly works for GET requests by default.

```java
package io.bubblesort.springbootstarter.hello;

import java.util.Arrays;
import java.util.List;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
	
	@RequestMapping("/hello")
	public List<String> sayHi() {
		return Arrays.asList( new String[] { "Java", "is", "awesome" } );
	}
}
```

```javascript
["Java","is","awesome"]
```
- The above code creates a RESTController that maps requests to /hello onto the sayHi() method.
- Notice that the conversion from Java world to JSON happens implicitly and is taken care of by Spring framework.

# Maven Configuration

`SpringApplication.run()` runs the Tomcat servlet container. This is done programmatically and the relevant Tomcat jar files are loaded into the project via the Maven repository.

![Maven Configuration](spring-auto-configuration.png)

Having Tomcat server embedded has the following advantages:
1. Convenience
2. Standalone application
3. Useful for Microservices architecture - no additional steps needed to configure multiple Microservices instances.
4. Servlet container is now part of Application itself and can be configured via Application configuration itself.

# Annotations

@SpringBootApplication
@RestController
@RequestMapping("/topics")

# Application.properties
- A configuration point for a Spring application.
- Ex: 
  - server.port=3000
  - spring.data.cassandra.username
  - spring.data.cassandra.password
  - management.port=9001
- What all properties are available for configuration is provided at: https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html

# Spring MVC Controller
- ![image](https://user-images.githubusercontent.com/42272776/137579022-040ca0de-6d6a-4992-8165-7cc9bc88bceb.png)

# SpringBoot Actuator
- Comes in Operations category that has features to give statistics on how the application is running - health, utilization etc.
- https://spring.io/guides/gs/actuator-service/

# 

# Examples
- The below code depicts a RestController that maps /topics URI to getAllTopics() method.
- The return value of that method gets serialzied into JSON content. This is handled for us by SpringMVC.

```
package io.firehose.springbootstarter.io.firehose.springbootstarter.topic;

import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Arrays;
import java.util.List;

@RestController
public class TopicController
{
    @RequestMapping("/topics")
    public List<Topic> getAllTopics()
    {
        return Arrays.asList(
                new Topic("Spring", "Spring Framework", "Stable Microservices"),
                new Topic("Cassandra", "Distributed Database", "Highavailable Fast Database"),
                new Topic("Redis", "Caching Service", "Performance Booster")
        );
    }
}
```

- When localhost:8080/topics is hit, the result would be that this getAllTopics would be hit and its response would be marked as the return JSON.
```
[{"id":"Spring","name":"Spring Framework","description":"Stable Microservices"},{"id":"Cassandra","name":"Distributed Database","description":"Highavailable Fast Database"},{"id":"Redis","name":"Caching Service","description":"Performance Booster"}]
```

# SpringBoot REST API
- You start by creating a Spring project.
  - So there would be a class annotated by **@SpringApplication**
  - Define Controller classes **@RestController** which Spring would use for mapping URL requests to API calls
  - Create API calls and map them to URIs using **@RequestMapping** annotations
  - Business services can be created to hold the singleton data using **@Service** and can be used in the controller methods.
  - **@Autowired** helps in populating the object in StringBoot application with the singleton instance.
## CRUD API
```
package io.firehose.springbootstarter.controllers;

import io.firehose.springbootstarter.topic.Topic;
import io.firehose.springbootstarter.services.TopicService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class TopicController
{
    @Autowired
    private TopicService topicService;

    @RequestMapping("/topics")
    public List<Topic> getAllTopics()
    {
        return topicService.getAllTopics();
    }

    @RequestMapping("/topics/{id}")
    public Topic getTopic(@PathVariable String id)
    {
        return topicService.getTopic(id);
    }

    @RequestMapping(method = RequestMethod.POST, value="/topics")
    public void addTopic(@RequestBody Topic topic)
    {
        topicService.addTopic(topic);
    }

    @RequestMapping(method = RequestMethod.PUT, value="/topics/{id}")
    public void updateTopic(@RequestBody Topic topic, @PathVariable String id)
    {
        topicService.updateTopic(id, topic);
    }

    @RequestMapping(method = RequestMethod.DELETE, value="/topics/{id}")
    public void deleteTopic(@PathVariable String id)
    {
        topicService.deleteTopic(id);
    }

}
```

# Database Connections
- JPA is Java Persistance Architecture, which is a specification. Spring Data JPA is the Spring implementation of the same.

# Introduction
Spring is a huge framework used for creating standalone Spring based applications easily. Spring is an application framework - that does the handling of Http requests, connections to databases ( RDBMS, MongoDB ), running queries etc. Sprint Boot is like a wrapper on top of it so that the setting of Spring project, working and managing its dependencies are easy. This is extremely useful because most projects have some commonalities like connecting to databases, etc. So having this supplied by default via the Spring application using Bootstrap is what Spring Boot does.

# Problems with Spring
1. Huge framework
2. Lot of steps involved in setting up a project
3. Configuration steps
4. Multiple build and deploy steps
These steps are abstracted via Sprint Boot.

# Creating a SpringBoot project

Spring applications are typically managed by Maven. You can just create a Maven project and add the following parent/dependency.

  `<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.2.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
  </dependencies>`

The `<parent>` section inherits the configurations defined in that Maven project onto the current project and the `<dependency>` section describes the dependencies needed for the current project to be build successfully.

# MAVEN

### Central Repo  https://repo.maven.apache.org/maven2/

- ### Quickstart Archetype

  - G  -------------  org.apache.maven.archetypes
  - A --------------- maven-archetype-quickstart
  - V -------------- 1.1

- ### web app Archetype

  - G  -------------  org.apache.maven.archetypes
  - A --------------- maven-archetype-webapp
  - V -------------- 1.0



```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.luv2code</groupId>
	<artifactId>spring-security-demo</artifactId>
	<version>1.0</version>
	<packaging>war</packaging>

	<name>spring-security-demo</name>

	<properties>
		<springframework.version>5.0.2.RELEASE</springframework.version>
		<springsecurity.version>5.0.0.RELEASE</springsecurity.version>

		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
	</properties>

	<dependencies>

		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>

	</dependencies>
    
    <repositories>
    	<repository>
        	<id></id>
            <name></name>
            <url></url>
        </repository>
    
    </repositories>

	<!-- TO DO: Add support for Maven WAR Plugin -->

	<build>
		<finalName>spring-security-demo</finalName>
	
		<pluginManagement>
			<plugins>
				<plugin>
					<!-- Add Maven coordinates (GAV) for: maven-war-plugin -->
				    <groupId>org.apache.maven.plugins</groupId>
				    <artifactId>maven-war-plugin</artifactId>
				    <version>3.2.0</version>					
				</plugin>						
			</plugins>
		</pluginManagement>
	</build>


</project>

```


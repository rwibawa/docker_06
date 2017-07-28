# docker_06
Spring Boot with Docker

# 1. Create a spring boot project
```bash
  $ vi pom.xml
  $ vi src/main/java/hello/Application.java
  $ mvn package
  $ java -jar target/gs-spring-boot-docker-0.1.0.jar
```
Open it at (http://localhost:8080)

# 2. Add the docker plugin to the pom.xml
```xml
<properties>
   <docker.image.prefix>springio</docker.image.prefix>
</properties>
<build>
    <plugins>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>dockerfile-maven-plugin</artifactId>
            <version>1.3.4</version>
            <configuration>
                <repository>${docker.image.prefix}/${project.artifactId}</repository>
            </configuration>
        </plugin>
    </plugins>
</build>
```

It will create a docker image "**springio/gs-spring-boot-docker**".

Create a __Dockerfile__ and build it with maven dockerfile plugin, then run the docker image:
```bash
  $ vi Dockerfile
  $ mvn install dockerfile:build
  $ docker images
  $ docker run -p 8080:8080 -t springio/gs-spring-boot-docker

  $ docker ps -a
  $ docker stop ba94367dbcc3
  $ docker rm ba94367dbcc3
```
Open it at (http://localhost:8080)

## 3. Using Spring profile
```bash
$ docker run -e "SPRING_PROFILES_ACTIVE=prod" -p 8080:8080 -t springio/gs-spring-boot-docker
$ docker run -e "SPRING_PROFILES_ACTIVE=dev" -p 8080:8080 -t springio/gs-spring-boot-docker
```

## 4. Run it in debug mode:
```bash
$ docker run -e "JAVA_OPTS=-agentlib:jdwp=transport=dt_socket,address=5005,server=y,suspend=n" -p 8080:8080 -p 5005:5005 -t springio/gs-spring-boot-docker
```

# Optimizing Image size with Multistage builds

the multistage build is a feature available in recent Docker Engine that enable images mimification by seperating build and runtime stages in docker files.



1. Update the dockerfile 

```
FROM maven:3-jdk-11-openj9  AS build
COPY ./gs-rest-service/complete/src  /usr/src/app/src
COPY ./gs-rest-service/complete/pom.xml /usr/src/app/pom.xml
RUN mvn -f /usr/src/app/pom.xml clean package


FROM openjdk:15-jdk-alpine3.11
COPY --from=build /usr/src/app/target/rest-service-0.0.1-SNAPSHOT.jar  /usr/app/rest-service-0.0.1-SNAPSHOT.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","/usr/app/rest-service-0.0.1-SNAPSHOT.jar"]
```

In this new dockerfile, we first produce a  jar in the build stage
then copy this jar in the runner stage with  `--from=build`


2. build the new package 
 ``` 
 docker build -f Dockerfile-onbuild -t nelvadas/greeting:2.0.0 .
```


3. compare image sizes

```
$ docker images | grep  nelvadas/greeting
nelvadas/greeting      2.0.0     66c71d10fd34   3 minutes ago  356MB
nelvadas/greeting      1.0.0     a3448443cad7   9 minutes ago  500MB
```


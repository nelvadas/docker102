FROM maven:3-jdk-11-openj9
COPY ./gs-rest-service/complete/src  /usr/src/app/src
COPY ./gs-rest-service/complete/pom.xml /usr/src/app/pom.xml 
EXPOSE 8080
RUN mvn -f /usr/src/app/pom.xml clean package
CMD java -jar /usr/src/app/target/rest-service-0.0.1-SNAPSHOT.jar


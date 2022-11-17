# First stage: complete build environment
FROM maven:3-amazoncorretto-8 AS builder
#FROM --platform=linux/amd64 amazonlinux:2018.03

# add pom.xml and source code
ADD ./pom.xml pom.xml
ADD ./src src/

# package jar
RUN mvn clean install

# Second stage: minimal runtime environment
From openjdk:8-jre-alpine

# copy jar from the first stage
COPY --from=builder target/multilender-0.0.1-SNAPSHOT.jar multilender-0.0.1-SNAPSHOT.jar

EXPOSE 8080

CMD ["java", "-jar", "multilender-0.0.1-SNAPSHOT.jar"]

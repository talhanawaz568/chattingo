# Use a base image
FROM maven:3.9.9-amazoncorretto-21 AS deps

WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline

#stage 2:
FROM maven:3.9.9-amazoncorretto-21 AS build
WORKDIR /app
COPY pom.xml .
COPY --from=deps /root/.m2 /root/.m2
COPY src ./src
RUN mvn clean package -DskipTests

FROM openjdk:21-jdk-slim

COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

CMD [ "java", "-jar","app.jar" ]

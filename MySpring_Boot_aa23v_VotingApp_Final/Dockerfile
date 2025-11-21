#buildtime
FROM maven:3.8.7-eclipse-temurin-17 AS build
LABEL owner="Edidiongakpan18@gmail.com" description="dockerfile for sprinboot voting app"
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests
#Runtime
FROM eclipse-temurin:17-jdk-focal
WORKDIR /app
COPY --from=build /app/target/MySpring_Boot_aa23v_VotingApp_Final-0.0.1-SNAPSHOT.jar /app/MySpring_Boot_aa23v_VotingApp_Final-0.0.1-SNAPSHOT.jar
VOLUME ["/app/logs/volume"]
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "MySpring_Boot_aa23v_VotingApp_Final-0.0.1-SNAPSHOT.jar"]

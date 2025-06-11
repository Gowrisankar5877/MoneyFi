# ---------- Stage 1: Build the JAR ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

# ---------- Stage 2: Create a minimal image ----------
FROM eclipse-temurin:17-jdk

WORKDIR /app

COPY --from=builder /app/target/*.jar app.jar

EXPOSE 8300

# Set the Spring profile
ENV SPRING_PROFILES_ACTIVE=sqlserver

ENTRYPOINT ["java", "-jar", "app.jar"]

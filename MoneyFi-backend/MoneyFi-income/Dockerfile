# ---------- Stage 1: Build the JAR ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

# Copy pom.xml first and resolve dependencies
COPY pom.xml .

RUN mvn dependency:go-offline

# Copy source files
COPY src ./src

# Build the Spring Boot application, skipping tests
RUN mvn clean package -DskipTests

# ---------- Stage 2: Minimal runtime image ----------
FROM eclipse-temurin:17-jdk

WORKDIR /app

# Copy the built JAR from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose port as defined in application-sqlserver.yml
EXPOSE 8100

# Run the application with dynamic profile
ENTRYPOINT ["sh", "-c", "java -jar app.jar"]

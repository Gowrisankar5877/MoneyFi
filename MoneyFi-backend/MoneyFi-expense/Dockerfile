# ---------- Stage 1: Build the JAR ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

WORKDIR /app

# Copy pom.xml and download dependencies (cached)
COPY pom.xml .

RUN mvn dependency:go-offline

# Copy source code
COPY src ./src

# Build the app, skip tests to speed up (remove -DskipTests if you want tests)
RUN mvn clean package -DskipTests

# ---------- Stage 2: Create minimal runtime image ----------
FROM eclipse-temurin:17-jdk

WORKDIR /app

# Copy the jar from builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose your app port
EXPOSE 8200

# Run the jar with active profile set from environment variable
ENTRYPOINT ["sh", "-c", "java -jar app.jar"]


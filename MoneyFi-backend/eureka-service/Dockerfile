# ---------- Stage 1: Build the JAR ----------
FROM maven:3.9.6-eclipse-temurin-17 AS builder

# Set working directory
WORKDIR /app

# Copy pom.xml and download dependencies (this layer gets cached)
COPY pom.xml .
# Copy the rest of the source code
COPY src ./src

# Build the application
RUN mvn clean package

# ---------- Stage 2: Create a minimal image ----------
FROM eclipse-temurin:17-jdk

# Set working directory
WORKDIR /app

# Copy JAR from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose port (if your app uses one)
EXPOSE 8761

# Run the application
CMD ["java", "-jar", "app.jar"]

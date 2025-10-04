# ===============================
# Step 1: Build Stage using Maven
# ===============================
FROM maven:3.9.3-eclipse-temurin-17 AS build

# Set working directory
WORKDIR /app

# Copy parent and module pom files
COPY pom.xml .
COPY jps-service/pom.xml ./jps-service/
COPY web-ser/pom.xml ./web-ser/

# Copy the full source code of both modules
COPY jps-service/src ./jps-service/src
COPY web-ser/src ./web-ser/src

# Package the Spring Boot app (skip tests for faster build)
RUN mvn clean package -DskipTests

# =================================
# Step 2: Run Stage using OpenJDK 17
# =================================
FROM openjdk:17-jdk-slim

WORKDIR /app

# Copy the final JAR from the build stage
COPY --from=build /app/web-ser/target/web-ser-0.0.1-SNAPSHOT.jar app.jar

# Expose the application port
EXPOSE 8080

# Run the application
ENTRYPOINT ["java", "-jar", "app.jar"]


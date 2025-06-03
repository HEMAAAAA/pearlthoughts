# Build stage
FROM maven:3.6.3-openjdk-17-slim AS builder

# Set working directory
WORKDIR /build

# Copy pom.xml separately for better caching
COPY pom.xml .
COPY src ./src

# Package the application
RUN mvn clean package -DskipTests

# Runtime stage - smaller JRE image
FROM openjdk:17-slim

# Set working directory
WORKDIR /app

# Copy JAR from builder stage
COPY --from=builder /build/target/*.jar app.jar

# Expose application port
EXPOSE 9090

# Run the application
CMD ["java", "-jar", "app.jar"]
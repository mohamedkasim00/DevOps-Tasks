# Stage 1: Build the Spring PetClinic application
FROM maven:3.8.7-eclipse-temurin-17 AS builder

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

# Stage 2: Set up the final image with MySQL and the Spring PetClinic application
FROM openjdk:17-jdk-alpine

# Install MySQL
RUN apk add --no-cache mysql mysql-client

# Set environment variables for MySQL
ENV MYSQL_ROOT_PASSWORD=rootpassword
ENV MYSQL_DATABASE=petclinic
ENV MYSQL_USER=petclinic
ENV MYSQL_PASSWORD=petclinicpassword

# Copy the Spring PetClinic JAR from the builder stage
COPY --from=builder /app/target/spring-petclinic*.jar /app/spring-petclinic.jar

# Copy the initialization script to the MySQL initialization directory
COPY init-db.sql /docker-entrypoint-initdb.d/

# Expose the necessary ports
EXPOSE 8080 3306

# Command to start both MySQL and the Spring PetClinic application
CMD ["sh", "-c", "mysqld_safe & java -jar /app/spring-petclinic.jar"]


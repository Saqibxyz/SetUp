# Spring Boot Application (Dockerized)

A lightweight Spring Boot application packaged into a Docker container.  
This project demonstrates how to containerize a Spring Boot app for easy deployment and portability.

---

## Project Structure

.
├── src/               
├── target/            
├── Dockerfile        
├── .dockerignore      
├── pom.xml           
└── README.md          

---

## Prerequisites

Make sure the following are installed on your system:

- Java 17+
- Maven 3.8+
- Docker

Check versions:

    java -version
    mvn -v
    docker --version

---

## Build & Run Locally (without Docker)

    # Build the application JAR
    mvn clean package -DskipTests

    # Run the JAR directly
    java -jar target/*.jar

The application will be available at: http://localhost:8080

---

## Build & Run with Docker

### 1. Build Docker Image
    docker build -t spring-boot-app:1.0 .

### 2. Run Docker Container
    docker run --rm -p 8080:8080 spring-boot-app:1.0

### 3. Verify
Visit: http://localhost:8080

---

## Dockerfile (Simple Version)

    FROM eclipse-temurin:17-jre-jammy

    WORKDIR /app
    COPY target/*.jar app.jar

    EXPOSE 8080
    ENTRYPOINT ["java", "-jar", "/app/app.jar"]

---

## Useful Docker Commands

    # List running containers
    docker ps

    # Stop container
    docker stop <container-id>

    # View logs
    docker logs -f <container-id>

    # Remove unused images/containers
    docker system prune -f

---

## Optional: Build Image with Maven (Spring Boot Buildpacks)

    mvn spring-boot:build-image -Dspring-boot.build-image.imageName=spring-boot-app:1.0

---

## Next Steps

- Add docker-compose.yml to run alongside a database (PostgreSQL, MySQL, etc.)
- Push your Docker image to Docker Hub or GitHub Container Registry
- Deploy to cloud platforms (AWS, GCP, Azure, Kubernetes)

---

## Author

Created with Spring Boot and Docker

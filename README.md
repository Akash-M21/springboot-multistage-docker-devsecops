# Spring Boot Multi-stage Docker Build

A Spring Boot application built using a **multi-stage Docker build** approach.

This project demonstrates how to:

* Build a Java application using Maven
* Separate build and runtime environments
* Reduce final Docker image size
* Scan application artifacts using Trivy
* Run the application using a lightweight Java runtime image

---

## Architecture

```
Developer Code
       |
       v
 Maven Dependency Stage
       |
       v
 Application Build Stage
       |
       v
 Security Scan Stage (Trivy)
       |
       v
 Runtime Stage (JRE)
       |
       v
 Docker Container
```

---

## Project Structure

```
spring-multistage-app/

├── Dockerfile
├── pom.xml
└── src
    └── main
        └── java
            └── com
                └── example
                    └── App.java
```

---

## Technologies Used

* Java 17
* Spring Boot
* Maven
* Docker
* Trivy

---

# Docker Multi-stage Build

The Dockerfile contains 4 stages:

## Stage 1: Dependencies

Downloads Maven dependencies.

```
maven:3.9.9-eclipse-temurin-17
```

Purpose:

* Improves Docker layer caching
* Avoids downloading dependencies repeatedly

---

## Stage 2: Build

Compiles the Spring Boot application.

Creates:

```
target/*.jar
```

---

## Stage 3: Security Scan

Uses Trivy to scan the generated application artifact.

Checks for:

* Vulnerabilities
* Security issues

---

## Stage 4: Runtime

Uses only:

```
eclipse-temurin:17-jre
```

Contains:

* Java runtime
* Application JAR

Does not contain:

* Maven
* Source code
* Build tools

---

# Build Docker Image

Clone repository:

```bash
git clone <repository-url>

cd spring-multistage-app
```

Build image:

```bash
docker build -t spring-app .
```

---

# Run Container

```bash
docker run -d \
-p 8080:8080 \
--name spring-container \
spring-app
```

---

# Access Application

Open:

```
http://localhost:8080
```

Expected response:

```
Spring Boot running inside Docker
```

---

# Useful Docker Commands

Check running containers:

```bash
docker ps
```

View logs:

```bash
docker logs spring-container
```

Stop container:

```bash
docker stop spring-container
```

Remove container:

```bash
docker rm spring-container
```

---

# Benefits of Multi-stage Build

| Normal Build         | Multi-stage Build      |
| -------------------- | ---------------------- |
| Large image          | Smaller image          |
| Contains build tools | Runtime only           |
| More attack surface  | Reduced attack surface |
| Slower deployment    | Faster deployment      |

---

## Author

AKASH M

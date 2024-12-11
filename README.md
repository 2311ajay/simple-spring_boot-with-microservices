# Simple Microservice Application Using Spring Boot and Spring Cloud

[![Spring Boot](https://github.com/2311ajay/simple-kafka-spring_boot/assets/92317294/2443afc6-d351-4c0a-b522-cfc828329643)](https://spring.io/)

## Overview

This project demonstrates a simple microservice-based application built with Spring Boot and Spring Cloud. The application showcases how microservices can interact with each other using REST APIs and a Spring Cloud API Gateway.

## Features

- **Service Discovery**: Uses Eureka for dynamic service registration and discovery.
- **API Gateway**: Implements Spring Cloud Gateway to route requests to the appropriate microservices.
- **Centralized Configuration**: Uses Spring Cloud Config for centralized property management across microservices.
- **Modular Services**: Two microservices (`student` and `school`) demonstrate how loosely coupled services communicate.

## Prerequisites

Before running the application, ensure that the following software is installed:

- Java Development Kit (JDK) 17 or later
- Maven (build tool)
- (optional but recommended) Insomnia/Postman or any other HTTP client for testing the endpoints
- Setup a postgres database with tables `schools` and `students` and update the files in `config-server/src/main/resources/configurations` accordingly.

## Microservice Architecture

### Components

1. **Gateway**: Serves as the entry point to route requests to the appropriate service.
2. **Student Service**: Manages student-related data.
3. **School Service**: Manages school-related data.
4. **Eureka Server**: Manages service registration and discovery.
5. **Config Server**: Centralized configuration repository for all services.

### Communication Flow

- Client sends a request to the API Gateway.
- Gateway routes the request to the appropriate microservice (`student` or `school`) based on the route configuration.
- Microservices retrieve configurations from the Config Server and register themselves with the Eureka Server.

## Setup

1. **Clone the Repository**:

   ```bash
   git clone <repository-url>
   cd <repository-directory>
   ```

2. **Start Eureka Server**:

    - Navigate to the `eureka-server` directory and start the Eureka server:
      ```bash
      mvn clean install
      mvn spring-boot:run
      ```

3. **Start Config Server**:

    - Navigate to the `config-server` directory, update its `application.yml` with the path to the configuration files, and run:
      ```bash
      mvn clean install
      mvn spring-boot:run
      ```

4. **Start Other Services**:

    - Start `gateway`, `student`, and `school` services by navigating to their respective directories and running:
      ```bash
      mvn clean install
      mvn spring-boot:run
      ```

5. **Test API Endpoints**:

    - Use Insomnia/Postman or curl to test endpoints via the API Gateway. Example:
        - `GET /api/v1/students`: Fetch students.
        - `GET /api/v1/schools`: Fetch schools.

## Configuration

### Gateway Routes

Routes are defined in the `gateway` service's `application.yml`:

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: students
          uri: http://localhost:8090
          predicates:
            - Path=/api/v1/students/**
        - id: schools
          uri: http://localhost:8070
          predicates:
            - Path=/api/v1/schools/**
```


## Service-Specific Details

### Student Service

- **Port**: 8090
- **Endpoints**:
    - `GET /api/v1/students`: Retrieve a list of students.
    - `POST /api/v1/students`: Create a new student.

### School Service

- **Port**: 8070
- **Endpoints**:
    - `GET /api/v1/schools`: Retrieve a list of schools.
    - `POST /api/v1/schools`: Create a new school.

## References

- [Spring Cloud Documentation](https://spring.io/projects/spring-cloud)
- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [A great tutorial on how to implement this simple project!](https://youtu.be/KJ0cSvYj41c?si=10g2VEe0ov1ETtnY)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.


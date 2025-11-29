# E-Learning Config Server

Spring Cloud Config Server for managing centralized configuration across all microservices in the e-learning platform.

## Project Structure

```
elearning-config-server/
├── config-server/
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/
│   │   │   │   └── ma/
│   │   │   │       └── elearning/
│   │   │   │           └── config_server/
│   │   │   │               └── ConfigServerApplication.java
│   │   │   └── resources/
│   │   │       ├── bootstrap.yml
│   │   │       └── application.yml
│   │   └── test/
│   │       └── java/
│   │           └── ma/
│   │               └── elearning/
│   │                   └── config_server/
│   │                       └── ConfigServerApplicationTests.java
│   ├── pom.xml
│   ├── mvnw
│   └── mvnw.cmd
├── README.md
└── QUICKSTART.md
```

## Technology Stack

- **Java**: 17
- **Spring Boot**: 3.3.5
- **Spring Cloud**: 2023.0.3
- **Dependencies**:
  - Spring Cloud Config Server
  - Spring Boot Actuator
  - Spring Web

## Configuration

The server runs on **port 8888** by default.

### Git Repository Configuration

The config server reads configurations from a **local Git repository**.

**IMPORTANT**: Before running, edit `src/main/resources/bootstrap.yml` and replace `PATH_TO_CONFIG_REPO` with the absolute path to your configuration repository.

Example:
```yaml
# Windows
uri: file:///C:/Users/user/desktop/elearning/elearning-config

# Linux/Mac
uri: file:///home/user/elearning/elearning-config
```

The configuration repository should be a Git repository containing your `.yml` or `.properties` files for each microservice.

## Your Configuration Repository

Your config repository (elearning-config) contains configurations for all your microservices:

- `application-default.yml` / `application-default-dev.yml` / `application-default-prod.yml`
- `analytics-service.yml` / `analytics-service-dev.yml` / `analytics-service-prod.yml`
- `coupon-service.yml` / `coupon-service-dev.yml` / `coupon-service-prod.yml`
- `course-service.yml` / `course-service-dev.yml` / `course-service-prod.yml`
- `enrollment-service.yml` / `enrollment-service-dev.yml` / `enrollment-service-prod.yml`
- `notification-service.yml` / `notification-service-dev.yml` / `notification-service-prod.yml`
- `payment-service.yml` / `payment-service-dev.yml` / `payment-service-prod.yml`
- `reporting-service.yml` / `reporting-service-dev.yml` / `reporting-service-prod.yml`
- `roadmap-service.yml` / `roadmap-service-dev.yml` / `roadmap-service-prod.yml`
- `user-service.yml` / `user-service-dev.yml` / `user-service-prod.yml`

## Running the Config Server

### Prerequisites
- Java 17 or higher
- Maven 3.6+
- Local Git repository with configuration files

### Step 1: Configure the Repository Path

Edit `config-server/src/main/resources/bootstrap.yml` and set your config repo path:

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: file:///C:/Users/user/desktop/elearning/elearning-config
```

### Step 2: Build and Run

**Windows (PowerShell):**
```powershell
cd config-server
.\mvnw.cmd clean install
.\mvnw.cmd spring-boot:run
```

**Linux/Mac:**
```bash
cd config-server
./mvnw clean install
./mvnw spring-boot:run
```

### Step 3: Verify Server is Running

```bash
curl http://localhost:8888/actuator/health
```

You should see: `{"status":"UP"}`

## Accessing Configurations

Configurations can be accessed via REST endpoints:

**Format**: `http://localhost:8888/{application}/{profile}`

Examples:
```bash
# Get default config for course-service
curl http://localhost:8888/course-service/default

# Get dev profile config for course-service
curl http://localhost:8888/course-service/dev

# Get prod profile config for payment-service
curl http://localhost:8888/payment-service/prod

# Get user-service config
curl http://localhost:8888/user-service/default
```

## Client Configuration

To connect your microservices to this config server:

### 1. Add dependency to each microservice's `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

### 2. Configure `bootstrap.yml` in each microservice:

```yaml
spring:
  application:
    name: course-service  # Change to your service name
  cloud:
    config:
      uri: http://localhost:8888
      fail-fast: false
  profiles:
    active: dev  # or prod
```

## Actuator Endpoints

Available monitoring endpoints:
- `/actuator/health` - Health check
- `/actuator/info` - Application info
- `/actuator/refresh` - Refresh configuration

## Actuator Endpoints

Available monitoring endpoints:
- `http://localhost:8888/actuator/health` - Health check
- `http://localhost:8888/actuator/info` - Application info
- `http://localhost:8888/actuator/env` - Environment properties

## Troubleshooting

### Error: Cannot clone or checkout repository

**Solution**: Verify the path in `bootstrap.yml` is correct and points to a valid Git repository.

```bash
# Check if the path is a git repository
cd /path/to/your/config-repo
git status
```

### Port 8888 already in use

**Solution**: Change the port in `bootstrap.yml`:
```yaml
server:
  port: 8889
```

### Clients cannot connect

**Solution**: 
1. Verify config server is running: `curl http://localhost:8888/actuator/health`
2. Check firewall settings
3. Ensure client bootstrap.yml has correct URI

## Complete Project Summary

✅ **Project Name**: elearning-config-server  
✅ **Java Version**: 17  
✅ **Spring Boot**: 3.3.5  
✅ **Spring Cloud**: 2023.0.3  
✅ **Port**: 8888  
✅ **Git Repository**: Local file system  
✅ **Dependencies**: Config Server, Actuator, Spring Web  

## License

See LICENSE.md for details.

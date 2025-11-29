# Quick Start Guide - E-Learning Config Server

## Prerequisites

- Java 17 installed
- Maven 3.6+ installed
- Git repository with your configuration files

## Step 1: Set Your Config Repository Path

Edit `config-server/src/main/resources/bootstrap.yml`:

```yaml
spring:
  cloud:
    config:
      server:
        git:
          uri: file:///C:/path/to/your/elearning-config  # CHANGE THIS!
```

Replace with your actual path (use forward slashes even on Windows).

## Step 2: Run the Config Server

### Windows (PowerShell):
```powershell
cd config-server
.\mvnw.cmd clean install
.\mvnw.cmd spring-boot:run
```

### Linux/Mac:
```bash
cd config-server
./mvnw clean install
./mvnw spring-boot:run
```

## Step 3: Verify It's Running

Open browser or run:
```bash
curl http://localhost:8888/actuator/health
```

Expected output: `{"status":"UP"}`

## Step 4: Test Configuration Retrieval

```bash
# Get course-service default config
curl http://localhost:8888/course-service/default

# Get course-service dev config
curl http://localhost:8888/course-service/dev

# Get payment-service prod config
curl http://localhost:8888/payment-service/prod
```

## Alternative: Run as JAR

```bash
cd config-server
./mvnw clean package
java -jar target/elearning-config-server-1.0.0.jar
```

## Next Steps

1. ✅ Config Server running on port 8888
2. Configure your microservices to connect to this config server
3. Add `spring-cloud-starter-config` dependency to each microservice
4. Create `bootstrap.yml` in each microservice pointing to this server

## Troubleshooting

**Problem**: Cannot find config repository  
**Solution**: Verify the `uri` path in `bootstrap.yml` is correct and is a Git repository

**Problem**: Port 8888 is already in use  
**Solution**: Change port in `bootstrap.yml` under `server.port`

## Project Structure Reference

```
config-server/
├── src/main/
│   ├── java/.../ConfigServerApplication.java  (Main class with @EnableConfigServer)
│   └── resources/
│       ├── bootstrap.yml    (Config server settings - port 8888, Git URI)
│       └── application.yml  (Actuator settings)
└── pom.xml                   (Spring Boot 3.3.5, Spring Cloud 2023.0.3)
```

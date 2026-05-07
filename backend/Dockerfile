# Build stage
FROM maven:3.9.9-eclipse-temurin-21 AS build
WORKDIR /app

COPY . .

# Build the project (checks if pom.xml is in current dir or in backend/ folder)
RUN if [ -f "pom.xml" ]; then \
        mvn -DskipTests package; \
    else \
        cd backend && mvn -DskipTests package && mkdir -p ../target && cp target/*.jar ../target/; \
    fi

# Runtime stage
FROM eclipse-temurin:21-jre
WORKDIR /app

# Copy the built jar
COPY --from=build /app/target/*.jar ./app.jar

ENV PORT=8081
EXPOSE 8081

CMD ["java", "-jar", "app.jar"]
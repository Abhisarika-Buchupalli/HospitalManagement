# ---------- Build stage ----------
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app

# Copy Maven config and download dependencies (cached layer)
COPY pom.xml .
RUN mvn dependency:go-offline

# Copy source and build the jar
COPY src ./src
RUN mvn clean package -DskipTests

# ---------- Runtime stage ----------
FROM eclipse-temurin:21-jre
WORKDIR /app

# Copy the built jar from build stage
COPY --from=build /app/target/*.jar app.jar

# Change this if your backend uses a different port
EXPOSE 8081

# Optional: database config as env vars
ENV SPRING_DATASOURCE_URL=jdbc:mysql://host.docker.internal:3306/hospital \
    SPRING_DATASOURCE_USERNAME=root \
    SPRING_DATASOURCE_PASSWORD=root \
    SERVER_PORT=8081

CMD ["java", "-jar", "app.jar"]

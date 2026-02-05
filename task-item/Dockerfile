# Step 1: Build stage
FROM maven:3.8.5-openjdk-17 AS build
WORKDIR /app
# Copy only the pom.xml first to fetch dependencies (efficient caching)
COPY pom.xml .
RUN mvn dependency:go-offline
# Copy the rest of the code and build the JAR
COPY . .
RUN mvn clean package -DskipTests
# Step 2: Run stage
FROM openjdk:17-jdk-slim
WORKDIR /app
# Copy the built JAR from the build stage
COPY --from=build /app/target/*.jar app.jar
# Render needs to communicate with your app on port 8080
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]

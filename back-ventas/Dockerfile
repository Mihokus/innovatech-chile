
FROM maven:3.9-eclipse-temurin-17 AS builder

WORKDIR /app

COPY pom.xml ./
RUN mvn dependency:go-offline -B

COPY src ./src
RUN mvn package -DskipTests -B

FROM eclipse-temurin:17-jre-alpine

WORKDIR /app

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=builder /app/target/*.jar app.jar

RUN chown appuser:appgroup app.jar

USER appuser

EXPOSE 8080

ENV DB_ENDPOINT=localhost \
    DB_PORT=3306 \
    DB_NAME=ventas_db \
    DB_USERNAME=root \
    DB_PASSWORD=password

ENTRYPOINT ["java", "-jar", "app.jar"]
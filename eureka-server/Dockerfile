FROM eclipse-temurin:21-jdk-alpine
LABEL maintainer="Jackson Whiteley"

RUN apk --no-cache add bash curl

WORKDIR /workspace/app
COPY target/*.jar eureka-server.jar

ENTRYPOINT ["java", "-jar", "eureka-server.jar"]
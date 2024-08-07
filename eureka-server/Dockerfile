#stage 1
#Start with a base image containing Java runtime
FROM openjdk:21-slim as build

# Add Maintainer Info
FROM eclipse-temurin:21-jdk-alpine
LABEL maintainer="Jackson Whiteley"

# The application's jar file
ARG JAR_FILE

# Add the application's jar to the container
COPY ${JAR_FILE} app.jar

#unpackage jar file
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf /app.jar)

#stage 2
#Same Java runtime
FROM openjdk:21-slim
RUN apk --no-cache add bash curl

#Add volume pointing to /tmp
VOLUME /tmp
WORKDIR /workspace/app
COPY target/*.jar eureka-server.jar

#Copy unpackaged application to new container
ARG DEPENDENCY=/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

#execute the application
ENTRYPOINT ["java","-cp","app:app/lib/*","com.smartHomeHub.eureka.EurekaServerApplication"]
ENTRYPOINT ["java", "-jar", "eureka-server.jar"]

###################################################################
##########Alternate###############
#FROM eclipse-temurin:21-jdk-alpine
#LABEL maintainer="Jackson Whiteley"
#
#RUN apk --no-cache add bash curl
#
#WORKDIR /workspace/app
#COPY target/*.jar eureka-server.jar
#
#ENTRYPOINT ["java", "-jar", "eureka-server.jar"]
FROM maven:3.8.4-openjdk-17
WORKDIR /app
COPY . .
RUN ["mvn", "clean", "package", "-DfinalName=app"]
RUN ["cp", "target/app.jar", "app.jar"]
ENTRYPOINT ["java", "-jar", "app.jar"]
#ARG JAR_FILE_PATH=target/*.jar
#COPY ${JAR_FILE_PATH} app.jar


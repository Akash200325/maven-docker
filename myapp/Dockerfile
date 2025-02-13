FROM maven:3.8.5-openjdk-17
WORKDIR /app
COPY . /app
RUN mvn clean package
CMD ["java", "-jar", "target/myapp-1.0-SNAPSHOT.jar"]

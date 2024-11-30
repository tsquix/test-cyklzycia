FROM eclipse-temurin:17-jdk AS build
WORKDIR /app

# Instalacja Mavena
RUN apt-get update && \
    apt-get install -y maven

# Kopiuj pom.xml i kod źródłowy
COPY pom.xml .
COPY src src

# Zbuduj projekt
RUN mvn clean package -DskipTests

# Etap finalny
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Skopiuj wait-for-it script
COPY wait-for-it.sh /wait-for-it.sh
RUN chmod +x /wait-for-it.sh

# Skopiuj zbudowany jar
COPY --from=build /app/target/*.jar app.jar

EXPOSE 8080

# Zmodyfikowany ENTRYPOINT aby poczekać na bazę danych
ENTRYPOINT ["/wait-for-it.sh", "mysql-service:3306", "--", "java", "-jar", "app.jar"]

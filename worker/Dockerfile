# Dockerfile pour le service worker - étape build
FROM maven:3.9.6-eclipse-temurin-21-alpine as builder

WORKDIR /app

# Copier les fichiers nécessaires pour la compilation
COPY pom.xml /app/
COPY src/ /app/src/

# Construire l'application avec Maven
RUN mvn dependency:resolve && mvn package

# Dockerfile pour le service worker - étape exécution
FROM eclipse-temurin:21-jre-alpine

WORKDIR /app

# Copier le fichier JAR généré depuis l'étape de build
COPY --from=builder /app/target/worker-jar-with-dependencies.jar /app/

# Exposer le port
EXPOSE 8080

# Démarrer l'application Java
CMD ["java", "-jar", "worker-jar-with-dependencies.jar"]
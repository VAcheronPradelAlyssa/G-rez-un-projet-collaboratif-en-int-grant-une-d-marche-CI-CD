# Étape 1: Build avec Maven
FROM openjdk:11-jdk-slim as build

# Installer Maven
RUN apt-get update && apt-get install -y maven

# Définir le répertoire de travail
WORKDIR /app

# Copier le fichier pom.xml
COPY pom.xml .

# Télécharger les dépendances (mise en cache)
RUN mvn dependency:go-offline -B

# Copier le code source
COPY src ./src

# Builder l'application
RUN mvn clean package -DskipTests

# Étape 2: Runtime avec Java
FROM openjdk:11-jre-slim

# Créer un utilisateur non-root
RUN addgroup --system spring && adduser --system spring --ingroup spring
USER spring:spring

# Définir le répertoire de travail
WORKDIR /app

# Copier le JAR depuis l'étape de build
COPY --from=build /app/target/*.jar app.jar

# Exposer le port
EXPOSE 8080

# Point d'entrée
ENTRYPOINT ["java", "-jar", "app.jar"]

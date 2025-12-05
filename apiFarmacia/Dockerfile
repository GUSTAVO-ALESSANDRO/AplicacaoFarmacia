# Etapa 1: Build com Maven e Java 25
FROM maven:3.9.6-eclipse-temurin-25 AS build
WORKDIR /app

# Copiar arquivos de configuração
COPY pom.xml .
COPY src ./src

# Baixar dependências e construir
RUN mvn clean package -DskipTests

# Etapa 2: Runtime com Java 25
FROM eclipse-temurin:25-jre
WORKDIR /app

# Copiar JAR do build
COPY --from=build /app/target/apiFarmacia-0.0.1-SNAPSHOT.jar app.jar

# Configurações
EXPOSE 8080

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

# Comando de execução
ENTRYPOINT ["java", "-jar", "app.jar"]
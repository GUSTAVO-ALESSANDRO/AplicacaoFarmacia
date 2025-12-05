# -----------------------------
# 1) Stage de Build (Maven)
# -----------------------------
FROM maven:3.9.6-eclipse-temurin-17 AS build

# Define diretório de trabalho dentro do container
WORKDIR /app

# Copia o arquivo pom.xml da pasta correta
COPY apiFarmacia/pom.xml .

# Baixa dependências antes do código (cache eficiente)
RUN mvn dependency:go-offline -B

# Copia todo o código fonte
COPY apiFarmacia/src ./src

# Compila e empacota a aplicação (gera o .jar)
RUN mvn clean package -DskipTests

# -----------------------------
# 2) Stage Final (Imagem Leve)
# -----------------------------
FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

# Copia apenas o JAR gerado no stage anterior
COPY --from=build /app/target/*.jar app.jar

# Exposição da porta padrão Spring Boot
EXPOSE 8080

# Variáveis de ambiente
ENV TZ="America/Sao_Paulo"

# Comando que executa a aplicação
ENTRYPOINT ["java", "-jar", "/app/app.jar"]

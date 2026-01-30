FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY . .
RUN ./mvnw -q -DskipTests package || true
CMD ["sh", "-c", "echo 'placeholder app container' && sleep 3600"]

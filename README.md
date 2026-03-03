# DevSecOps
DevSecOps Repository

## Running a Java-based container

This project can be packaged as a Java JAR and run inside a Docker container. Below are concise instructions for building the JAR, creating a Docker image, and running it.

Prerequisites:
- Docker installed and running
- A built JAR (for Maven: `mvn -DskipTests package`)

1) Example Dockerfile

```dockerfile
FROM eclipse-temurin:17-jdk
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} /app/app.jar
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

2) Build the app (Maven example)

```bash
mvn -DskipTests package
```

3) Build the Docker image

```bash
docker build -t myapp:latest .
```

4) Run the container

```bash
docker run -d --name myapp -p 8080:8080 myapp:latest
```

Alternative: run without building an image (mount the JAR into an OpenJDK image)

```bash
docker run --rm -v "$(pwd)/target/your-app.jar:/app/app.jar" -p 8080:8080 eclipse-temurin:17-jdk java -jar /app/app.jar
```

Notes:
- Replace `your-app.jar` or `target/*.jar` with your actual built JAR filename.
- Adjust `-p` port mapping and environment variables as needed (use `-e VAR=value`).
- For Apple Silicon (M1/M2) you can use multi-arch base images or add `--platform linux/amd64` to `docker build`/`docker run` if required.

Optional docker-compose (minimal)

```yaml
version: '3.8'
services:
	app:
		build: .
		image: myapp:latest
		ports:
			- "8080:8080"
		environment:
			- JAVA_OPTS=-Xmx256m
```


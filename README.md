# Real-time Chat: Docker + Jenkins CI/CD

A DevOps class project. I took a small open-source Node.js chat app ([amalan01/NodejsChatApp](https://github.com/amalan01/NodejsChatApp): Express, Socket.IO, Jade) and built a container and CI/CD pipeline around it. The pipeline is my work. The chat app itself is not.

## Pipeline

The `Jenkinsfile` runs on every build:

1. **Checkout** the repo
2. **Snyk:** scan dependencies for known vulnerabilities (fails on critical)
3. **SonarQube:** static code analysis
4. **Build** a Docker image (`griffinh00/nodejschatapp_repo`)
5. **Push** it to Docker Hub
6. **Deploy:** `docker-compose down` / `up -d` on the app server

Credentials for Docker Hub, Snyk, and SonarQube live in Jenkins, not in this repo.

## Files

```
Dockerfile           Node on Alpine, serves on port 3700
docker-compose.yml   Runs the published image, maps port 80 → 3700
Jenkinsfile          The pipeline above
index.js, public/, views/   The chat app
```

## Running it

```bash
docker build -t chatapp .
docker run -p 3700:3700 chatapp
```

Or without Docker: `npm install && node index.js`, then open http://localhost:3700.

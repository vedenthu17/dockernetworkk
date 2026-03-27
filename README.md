# Docker Network Example

This project demonstrates how multiple containers communicate with each other using a **custom Docker network**. The setup includes:

* Node.js backend (TypeScript)
* Redis container
* PostgreSQL container
* Shared Docker network

---

## 🧱 Architecture

```
app-net
 ├── backend (Node.js)
 ├── redis
 └── db (Postgres)
```

The backend connects to:

* Redis → `redis:6379`
* Postgres → `db:5432`

Docker automatically resolves container names as hostnames.

---

## 📁 Project Structure

```
.
├── src/
├── dist/
├── Dockerfile
├── docker-compose.yml
├── package.json
├── tsconfig.json
└── README.md
```

---

## 🚀 Build Docker Image

```bash
docker build -t ts-app .
```

---

## 🌐 Create Network

```bash
docker network create app-net
```

---

## 🟥 Run Redis

```bash
docker run -d --name redis --network app-net redis:7-alpine
```

---

## 🐘 Run PostgreSQL

```bash
docker run -d --name db \
-e POSTGRES_PASSWORD=postgres \
-e POSTGRES_USER=postgres \
-e POSTGRES_DB=postgres \
--network app-net postgres:16
```

Windows (single line):

```bash
docker run -d --name db -e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres -e POSTGRES_DB=postgres --network app-net postgres:16
```

---

## 🟢 Run Backend

```bash
docker run -it -p 3000:8000 --network app-net ts-app
```

---

## 🧪 Test Server

Open in browser:

```
http://localhost:3000
```

Expected response:

```json
{
  "message": "Hello from Typescript Server"
}
```

---

## 🔌 Container Communication

Backend connects using service names:

```ts
redis://redis:6379
```

```ts
host: "db"
```

Docker DNS resolves:

* `redis` → Redis container
* `db` → Postgres container

---

## 🛑 Stop Containers

```bash
docker stop redis db
```

---

## 🧹 Remove Containers

```bash
docker rm redis db
```

---

## 🧼 Remove Network

```bash
docker network rm app-net
```

---

## 🐳 Multi-stage Dockerfile

This project uses a **multi-stage build**:

* builder → compiles TypeScript
* runner → runs production JS
* smaller final image
* non-root user security

---

## 📦 Tech Stack

* Node.js
* TypeScript
* Express
* Redis
* PostgreSQL
* Docker
* Docker Networking

---

## 🎯 Goal

Learn:

* Docker custom networks
* container-to-container communication
* multi-container setup
* multi-stage Docker builds
* production Docker best practices

---

## Author

Vedant Tamhanekar

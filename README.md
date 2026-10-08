# Docker Practice and Learning Notes

A beginner-friendly, step-by-step collection of **Docker notes** (PDF) with diagrams, commands and worked examples. It goes from "what is a container?" to Dockerfiles, networking, volumes, Docker Compose and deploying a real website on Nginx.

> Built while learning Docker. Every topic has a short explanation, a runnable example and, where it helps, a diagram.

---

## Contents

| # | Notes (PDF) | What you will learn |
|---|-------------|---------------------|
| 1 | [Docker_Notes_Organized.pdf](notes/Docker_Notes_Organized.pdf) | Docker architecture (client, daemon, host, registry), container lifecycle, and the core commands: `pull`, `images`, `ps`, `create`, `start`, `stop`, `run`, `exec`, `logs`, `inspect`, `rm`, `rmi`, `image prune` |
| 2 | [Docker_Notes_Set3_Dockerfile.pdf](notes/Docker_Notes_Set3_Dockerfile.pdf) | Writing a Dockerfile: `FROM`, `RUN`, `CMD`, `ENV`, `COPY`, `WORKDIR`, `EXPOSE`, image layers, `docker history`, `docker build` and the build context |
| 3 | [Docker_Notes_Set4_Registry_Networking.pdf](notes/Docker_Notes_Set4_Registry_Networking.pdf) | Layer caching best practices, container runtime, image tags, `docker tag` / `login` / `push`, image lifecycle, bridge networking and `docker network` commands |
| 4 | [Docker_Notes_Set5_Volumes_Compose.pdf](notes/Docker_Notes_Set5_Volumes_Compose.pdf) | Volumes and bind mounts, Docker Compose (file structure, commands, networking), deploying a website on Nginx |

**Suggested order:** read them 1 to 4. Each part builds on the previous one.

---

## Repository structure

```
docker_practice/
├── README.md
└── notes/
    ├── Docker_Notes_Organized.pdf
    ├── Docker_Notes_Set3_Dockerfile.pdf
    ├── Docker_Notes_Set4_Registry_Networking.pdf
    └── Docker_Notes_Set5_Volumes_Compose.pdf
```

---

## Topics at a glance

### 1. Docker basics and commands
- Dockerfile → image → container
- Client, daemon, images, containers and registries
- Container lifecycle: create → start → stop → rm (`run` = create + start)
- Interactive shells with `docker exec -it`

### 2. Dockerfile and image build
- Core instructions and what runs at **build time** vs **run time**
- Image layers and `docker history`
- `docker build -t name:tag .` and custom Dockerfiles with `-f`
- `COPY` and `WORKDIR` explained with a complete Python example

### 3. Registries, lifecycle and networking
- Ordering a Dockerfile for fast cached builds
- Naming convention: `[registry/][username/]repository[:tag]`
- Build → tag → login → push → pull
- Bridge networks: `docker0`, `veth`, NAT, multiple bridges, user-defined networks

### 4. Volumes, Compose and deployment
- Volumes vs bind mounts and when to use each
- `docker-compose.yml`: services, image, build, ports, volumes, networks
- `docker compose up -d` / `down` and the default Compose network
- Hosting a static website with Nginx (Dockerfile and Compose)

---

## Quick start

**Prerequisite:** [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows / macOS) or Docker Engine (Linux).

```bash
docker --version
docker run hello-world
```

### Try it: run Nginx in 3 commands

```bash
docker pull nginx
docker run -d --name web -p 8080:80 nginx
# open http://localhost:8080 in your browser

docker stop web && docker rm web      # clean up
```

### Try it: build your own image

```dockerfile
# Dockerfile
FROM ubuntu
RUN apt-get update
CMD ["echo", "Hello from Docker"]
```

```bash
docker build -t hello-docker .
docker run --rm hello-docker
```

### Try it: keep data with a volume

```bash
docker run -it --name c1 -v mydata:/app/data ubuntu bash
# inside: echo "hello" > /app/data/note.txt, then exit
docker rm c1
docker run --rm -v mydata:/app/data ubuntu cat /app/data/note.txt   # prints: hello
```

### Try it: two services with Compose

```yaml
# docker-compose.yml
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - db-data:/var/lib/mysql

volumes:
  db-data:
```

```bash
docker compose up -d
docker compose ps
docker compose down
```

---

## Command cheat sheet

| Goal | Command |
|------|---------|
| Download an image | `docker pull IMAGE[:tag]` |
| List images / containers | `docker images` / `docker ps -a` |
| Run a container | `docker run -d --name NAME -p HOST:CONTAINER IMAGE` |
| Shell inside a container | `docker exec -it NAME /bin/bash` |
| View logs | `docker logs -f NAME` |
| Build an image | `docker build -t NAME:TAG .` |
| Tag and push | `docker tag IMAGE USER/REPO:TAG` then `docker push USER/REPO:TAG` |
| Create a network | `docker network create NAME` |
| Create a volume | `docker volume create NAME` |
| Start / stop a Compose app | `docker compose up -d` / `docker compose down` |
| Clean up | `docker rm NAME`, `docker rmi IMAGE`, `docker image prune` |

---

## How to use this repo

1. Clone it:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```
2. Open the PDFs in `notes/` in order.
3. Type the examples yourself in a terminal; practice beats reading.
4. Experiment: change a port, add a volume, break something and fix it.

---

## Author

**Aditya**, B.Tech Computer Science & Engineering student.

Notes are written for learning purposes. Suggestions and corrections are welcome: open an issue or a pull request.

## License

Add a license of your choice (for example [MIT](https://choosealicense.com/licenses/mit/)) so others know how they can reuse these notes.

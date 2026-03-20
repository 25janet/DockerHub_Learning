

---

# Docker Basics & Common Commands

This repository contains basic Docker commands and explanations for managing containers and images.

---

##  Docker Images

### List images

```bash
docker images
```

Shows all locally available images.

### Download an image

```bash
docker pull <image_name>
```

Downloads an image from DockerHub.

### Remove an image

```bash
docker rmi <image_name>
```

Deletes an image from your system.

---

##  Docker Containers

### Run a container

```bash
docker run <image_name>
```

Creates and starts a container from an image.

 Example:

```bash
docker run ubuntu
```

---

### List running containers

```bash
docker ps
```

### List all containers (including stopped ones)

```bash
docker ps -a
```

---

### Remove a container

```bash
docker rm <container_id>
```

---

### Stop a running container

```bash
docker stop <container_id>
```

---

### Execute a command inside a running container

```bash
docker exec <container_id> <command>
```

---

##  Running Containers (Important Options)

### Run interactively

```bash
docker run -it ubuntu
```

* `-i` → Keeps STDIN open (interactive input)
* `-t` → Allocates a terminal

 Used when you want to interact with the container (like a shell).

---

### Run in background (detached mode)

```bash
docker run -d ubuntu
```

* `-d` → Runs container in the background

---

### Keep container running

```bash
docker run -it ubuntu
```

If no process is running, the container exits. Interactive mode keeps it alive.

---

##  Naming Containers

```bash
docker run --name my_container ubuntu
```

* `--name` → Assigns a custom name to the container

 Easier than using container IDs.

---

## 🔌 Port Mapping (VERY IMPORTANT)

### What is Port Mapping?

Port mapping allows you to access a container’s service from your local machine.

---

### Syntax

```bash
docker run -p <host_port>:<container_port> <image>
```

 Example:

```bash
docker run -p 8080:80 nginx
```

* `8080` → Port on your computer (host)
* `80` → Port inside the container

 Now you can access the app in your browser:

```
http://localhost:8080
```

---

### Using `--publish`

```bash
docker run --publish 8080:80 nginx
```

* `-p` and `--publish` mean the same thing

---

##  Multiple Port Mapping

You can map multiple ports by repeating `-p`:

```bash
docker run -p 8080:80 -p 3000:3000 <image>
```

👉 Example:

```bash
docker run -p 8080:80 -p 443:443 nginx
```

---

##  Understanding `docker run` with Options

### Full Example

```bash
docker run -it -d --name my_app -p 8080:80 nginx
```

### Explanation:

* `-it` → Interactive terminal
* `-d` → Run in background
* `--name my_app` → Name of container
* `-p 8080:80` → Port mapping
* `nginx` → Image used

---

##  Advanced Commands

### Inspect a container

```bash
docker inspect <container_id>
```

Shows detailed information about the container.

---

### Stop all running containers

```bash
docker stop $(docker ps -q)
```

---

### Remove all containers

```bash
docker rm $(docker ps -aq)
```

---

##  Notes

* A container stops when its main process ends.
* Use `-it` for interactive work.
* Use `-d` for background services.
* Port mapping is required to access services from outside the container.

---



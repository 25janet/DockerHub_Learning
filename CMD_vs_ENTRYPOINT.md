
---

# Docker Fundamentals: CMD vs. ENTRYPOINT

This repository serves as a cheat sheet for understanding how Docker handles container startup commands, specifically the nuances between `CMD` and `ENTRYPOINT`.

## 🚀 The Basics
Both instructions define what command runs when a container starts, but they behave differently when you pass arguments during `docker run`.

### 1. CMD (The Default)
`CMD` provides a default command or parameters. It is **easily overridden** by any arguments provided at the command line.

* **In Dockerfile:**
    ```dockerfile
    CMD ["python", "app.py"]
    ```
* **Default Run:** `docker run my-image` executes `python app.py`.
* **Overridden Run:** `docker run my-image bash` executes `bash`. The `python app.py` part is entirely ignored.

### 2. ENTRYPOINT (The Executable)
`ENTRYPOINT` allows you to configure a container that will run as an executable. It is **harder to override** and is generally used for the main command of the image.

* **In Dockerfile:**
    ```dockerfile
    ENTRYPOINT ["sleep"]
    CMD ["5"]
    ```
* **The Combo:** When used together, `CMD` acts as the default argument for the `ENTRYPOINT`.
* **Running with arguments:** `docker run my-image 10` will result in the command `sleep 10`.

---

## 🛠 Practical Examples

### Overriding the Entrypoint
If you absolutely need to change the entrypoint of a running container, you must use the `--entrypoint` flag:
```bash
docker run --entrypoint sleep 2.0 ubuntu-sleeper 10
```

### Investigating Images
To find the configuration of existing Dockerfiles/images:
* **Find Entrypoint:** `cat Dockerfile-mysql`
* **Find CMD:** Use `nano Dockerfile-wordpress` (or `docker inspect`)

### Running Background Instances
To run an instance in detached mode with a specific command:
```bash
docker run -d ubuntu sleep 100
```

---

## 💾 Docker Storage & Networking
* **Ephemeral Nature:** Everything inside a Docker container is ephemeral. If the container is deleted, the data disappears with it.
* **Networking:** Use `host.docker.internal` to communicate from a container back to the host machine.

---

### 💡 Summary Table

| Feature | CMD | ENTRYPOINT |
| :--- | :--- | :--- |
| **Purpose** | Sets default commands/params | Sets the main executable |
| **Overriding** | Overwritten by arguments after image name | Appends arguments to the entrypoint |
| **Flexibility** | High (easy to swap commands) | Low (intended for specific tools) |


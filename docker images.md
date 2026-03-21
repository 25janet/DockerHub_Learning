
***

# Docker Images & Dockerfile Labs

This guide covers the creation and management of Docker images, including image inspection, building from a Dockerfile, and optimizing image size using Alpine Linux.

---

## 🖼️ Image Management Basics
Before building, it is important to know how to inspect what is already on your host.

* **Check available images:**
  ```bash
  docker images
  ```
* **Check the size of a specific image (e.g., Ubuntu):**
  Check the `SIZE` column in the `docker images` output.
* **Identify specific tags:**
  Find the tag for the latest created image (e.g., `nginx:latest`).

---

## 🛠️ Building Your Own Image
A **Dockerfile** is a text document that contains all the commands a user could call on the command line to assemble an image.

### Dockerfile Format
The basic structure follows: `INSTRUCTION argument`

### Example: Python Flask Web App
This Dockerfile sets up a environment for a Flask application:

```dockerfile
# Step 1: Base OS/Runtime
FROM python:3.6

# Step 2: Install dependencies
RUN pip install flask

# Step 3: Map working directory and copy code
WORKDIR /opt
COPY . /opt

# Step 4: Define network port
EXPOSE 8080

# Step 5: Set execution command
ENTRYPOINT ["python", "app.py"]
```

**Build Command:**
```bash
# Build and tag the image as 'webapp'
docker build -t webapp .
```

**Run Command:**
```bash
# Map host port 8282 to container port 8080
docker run -d -p 8282:8080 webapp-color
```

---

## ⚡ Image Optimization (Lighter Images)
Standard images (like `python:3.6`) can be quite large (often over 150 MB). To create a **"Lite"** version, we use **Alpine Linux** as a base.

### Modifying for Size:
1.  **Edit the Dockerfile:** Change `FROM python:3.6` to `FROM python:3.6-alpine`.
2.  **Rebuild:**
    ```bash
    docker build -t webapp-color:lite .
    ```
3.  **Deploy:**
    ```bash
    docker run -d -p 8383:8080 webapp-color:lite
    ```

---

## 🔍 Troubleshooting & OS Inspection
Sometimes you need to know what Operating System a base image is using (e.g., Debian vs. Alpine).

* **Check the Base OS version:**
  Run a temporary container and read the release file:
  ```bash
  docker run -it --rm python:3.6 cat /etc/*release*
  ```

---

## 📝 Key Instructions Summary
| Instruction | Purpose |
| :--- | :--- |
| **FROM** | Defines the base Operating System or Runtime. |
| **RUN** | Executes commands during the build phase (e.g., installing packages). |
| **COPY** | Moves files from the host machine into the container image. |
| **WORKDIR** | Sets the "Home" directory for any subsequent instructions. |
| **ENTRYPOINT** | Configures the container to run as an executable. |


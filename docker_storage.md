
---

# Docker Storage & Data Persistence

By default, data created within a Docker container is ephemeral. This guide explores how Docker manages its file system and the three main methods for ensuring data survives container termination.

## 🏗 The Docker File System

Docker uses a **Storage Driver** to manage the contents of the image layers and the writable container layer.

* **Read-Only Layers:** The layers that make up the Docker image.
* **Thin Writable Layer:** When a container is started, Docker adds a thin writable layer on top of the image layers.
* **The Catch:** This layer is **tightly coupled** to the container. If the container is removed (`rm`), the writable layer is wiped, and all data stored there is lost.


---

## 💾 Persistence Mechanisms

To save data permanently, Docker provides three primary options:

### 1. Volumes
The **preferred mechanism** for persisting data. Volumes are stored in a part of the host filesystem that is **managed by Docker** (`/var/lib/docker/volumes/` on Linux).
* **Independence:** They are completely independent of the container lifecycle.
* **Usage:**
    ```bash
    # Create a volume
    docker volume create my-data-db

    # Run a container using the volume
    docker run -d --name mysql-db -v my-data-db:/var/lib/mysql mysql
    ```

### 2. Bind Mounts
Maps a **specific path** on your host machine to a path inside the container.
* **Control:** You decide exactly where on the host the data lives.
* **Usage:**
    ```bash
    docker run -d -v /home/user/project/app:/app nginx
    ```

### 3. tmpfs Mounts
Stored in the **host system memory (RAM)** only.
* **Speed:** Extremely fast but non-persistent.
* **Security:** Data is never written to the host's hard drive.

---

## 🧪 Storage Test: The MySQL Disaster Recovery Scenario

To understand why this matters, consider a database deployment:

### Phase 1: The Ephemeral Failure
1.  Run a MySQL container without a volume:
    ```bash
    docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=db-pass123 mysql:5.7
    ```
2.  Add data to the database.
3.  **The Disaster:** If the container crashes or is deleted, all your database records are gone.

### Phase 2: The Solution (Using Bind Mounts/Volumes)
By mapping a host directory to the MySQL data directory (`/var/lib/mysql`), the data persists even if the container is destroyed.

**Command:**
```bash
docker run -d \
  --name mysql-db \
  -e MYSQL_ROOT_PASSWORD=db-pass123 \
  -v /opt/data:/var/lib/mysql \
  mysql:5.7
```

**Recovery:**
If the database crashes again, simply redeploy using the same command. Docker will reconnect to the data stored at `/opt/data`, and your records will be exactly where you left them.

---

## 🔍 Inspection Commands
* **Check container layers:** `docker inspect [container_id] | grep -i "UpperDir"`
* **List volumes:** `docker volume ls`
* **Verify data count:** `ls /root/sql-data | wc -l`


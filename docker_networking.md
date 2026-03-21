
---

# 🐳 Docker Networking & Storage Cheat Sheet

A guide to understanding how Docker containers communicate with each other, the host, and the internet, as well as how they handle data.

## 🌐 Network Types

Docker uses network drivers to manage container communication. The three main default networks are:

| Network Type | Description | Isolation Level |
| :--- | :--- | :--- |
| **Bridge** | The default network. Containers on the same bridge can talk via IP addresses. | Standard |
| **None** | Total isolation. The container has no external network interface. | Maximum |
| **Host** | The container shares the host's networking namespace directly. | None |

---

## 🌉 User-Defined Bridge Networks

While the default bridge requires IP addresses for communication, **User-Defined Bridges** provide **Automatic Service Discovery**.

* **Service Discovery:** Containers can connect to each other using their **name** instead of an IP address.
* **Example:** A web app named `B` can simply connect to a database container named `A` using the hostname `A`.

### Workflow Example:
1.  **Create the network:**
    ```bash
    docker network create my-app-net
    ```
2.  **Run containers on that network:**
    ```bash
    docker run -d --name mysql-db --network my-app-net mysql
    docker run -d --name webapp-color --network my-app-net webapp-color
    ```

---

## 🔌 Connection Mapping Reference

| Connection | Method / Tool |
| :--- | :--- |
| **Container ↔ Host** | Use `host.docker.internal` |
| **Container → Internet** | Handled via NAT |
| **Container ↔ Container** | Use **Custom Bridge Networks** (DNS by name) |
| **Internet → Container** | Use **Port Mapping**: `-p [Host_Port]:[Container_Port]` |

---

## 🛠 Useful Network Commands

| Command | Purpose |
| :--- | :--- |
| `docker network ls` | List all networks and identify how many exist. |
| `docker network inspect [net_name]` | Identify ID, subnet config, and attached containers. |
| `docker network connect` | Attach a running container to a network. |
| `docker network disconnect` | Remove a container from a network. |
| `docker network prune` | Remove all unused networks. |

---

## 🚀 Practical Deployment: MySQL & Web App

### 1. Create a custom bridge with specific IP ranges
```bash
docker network create \
  --driver bridge \
  --subnet 182.18.0.0/24 \
  --gateway 182.18.0.1 \
  wp-mysql-network
```

### 2. Deploy MySQL Database
```bash
docker run -d \
  --name mysql-db \
  --network wp-mysql-network \
  -e MYSQL_ROOT_PASSWORD=db-password123 \
  mysql:5.7
```

### 3. Deploy Web Application
Expose container port `8080` to host port `83080` and link to the DB:
```bash
docker run -d \
  --name webapp \
  -p 83080:8080 \
  --network wp-mysql-network \
  -e DB_Host=mysql-db \
  -e DB_Password=db-password123 \
  webapp-image
```

---


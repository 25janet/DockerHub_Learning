This third GitHub `README.md` covers **Environment Variables, Database Deployment, and the critical difference between CMD and ENTRYPOINT.**

***

# Docker: Environment Variables & Container Configuration

This guide explores how to configure containers dynamically using environment variables and covers the deployment of database containers with specific root credentials.

---

## 🔑 Environment Variables (`ENV`)
Environment variables allow you to change the behavior of an application without modifying the image or the code.

### Setting Variables at Runtime
Use the `-e` flag to pass environment variables into your container.
```bash
docker run -e APP_COLOR=blue kodekloud/simple-web-app
```

### Inspecting Variables
To verify which environment variables are active in a running container:
1. Find the Container ID using `docker ps`.
2. Run the inspect command:
   ```bash
   docker inspect [NAME/ID]
   ```
3. Scroll to the **Config** section; you will find the `Env` list containing all defined variables.

---

## 🗄️ Deploying a MySQL Database
When deploying databases like MySQL, you must set specific environment variables (like the root password) for the container to initialize properly.

**Task:** Deploy a MySQL database named `mysql-db` with a custom root password.

```bash
docker run -d --name mysql-db -e MYSQL_ROOT_PASSWORD=pass123 mysql
```

### Accessing the Database Shell
If you need to enter the container to create a database or run SQL queries:
```bash
docker exec -it mysql-db mysql -u root -p
```

---



> **Design Principle:** A container is designed to do exactly **one thing**.


---

## 🚀 Hands-on Exercise: Web App Deployment
Run a container named `blue-app` using the `kodekloud/simple-web-app` image with the following requirements:
* **Port Mapping:** Host `8828` to Container `8080`.
* **Environment Var:** Set `APP_COLOR` to `blue`.

```bash
docker run -d --name blue-app -p 8828:8080 -e APP_COLOR=blue kodekloud/simple-web-app
```

***


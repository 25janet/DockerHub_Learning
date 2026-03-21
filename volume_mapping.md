

## 1. Volume Mapping
Docker containers are **ephemeral**, meaning data created inside them is lost if the container is deleted. Volume mapping allows you to "pin" a folder inside the container to your host machine so data survives container restarts.

### Types of Mapping
* **Bind Mapping:** Maps a specific, existing folder on your computer directly into the container.
    * `docker run -it -v [Host Path]:[Container Path] [Image]`
* **Named Volumes:** Instead of a specific path, you provide a name. Docker manages where the files are stored on the host.
    * `docker run -d --name my-db -v db_data:/var/lib/mysql mysql`

---

## 2. Docker Images
Images are the read-only templates used to create containers.

### Key Commands & Operations
* **Build an Image:** Uses a `Dockerfile` to create a custom image.
    * `docker build -t webapp-color .`
* **Inspect Image:** Find details like the base OS (e.g., Python 3.6).
    * `docker inspect webapp-color`
* **Management:**
    * View local images: `docker images`
    * Check image size: `docker images --size`
    * Check tags: `docker images` (displays the repository and tag/version)

---

## 3. Environment Variables
Environment variables allow you to configure applications without changing the code or the image.

* **Setting Variables:** Use the `-e` flag during `docker run`.
    * `docker run -e APP_COLOR=blue webapp-color`
* **Inspecting Variables:** To see what variables are currently set in a running container:
    * `docker inspect [Container_ID]` (Look under the "Env" section).

---

## 4. CMD vs. ENTRYPOINT
Both instructions define the command that runs when a container starts, but they behave differently.

| Instruction | Behavior | Flexibility |
| :--- | :--- | :--- |
| **CMD** | The default command/parameters. | Easily overridden by adding arguments to `docker run`. |
| **ENTRYPOINT** | The fixed executable of the container. | Harder to override; arguments passed to `docker run` are appended to it. |

**Pro Tip:** You can use them together. Use `ENTRYPOINT` for the main command and `CMD` for the default parameters that a user might want to change.

---

## 5. Networking
Docker allows containers to communicate with each other and the outside world.

* **Bridge (Default):** The standard network for standalone containers. They can talk to each other via IP addresses.
* **User-Defined Bridge:** Recommended for production. Provides **Automatic Service Discovery**, allowing containers to communicate using their **container names** instead of IP addresses.
    * `docker network create my-app-net`
    * `docker run --network my-app-net --name mysql ...`
* **None/Null:** Total isolation; the container has no external network interface.
* **Host:** The container shares the host's network stack directly (removes isolation).

---

## 6. Storage
Docker uses a **Storage Driver** (like Overlay2, AUFS, or BTRFS) to manage the layers of an image and the writable layer of a container.

### The Layered File System
* **Image Layers:** Read-only layers created during the build process.
* **Writable Layer:** A thin layer added on top when a container runs. All changes (new files, modified files) are stored here.
* **Persistence Strategy:**
    1.  **Volumes:** Stored in a part of the host filesystem managed by Docker (`/var/lib/docker/volumes/`). Best for persistent data.
    2.  **Bind Mounts:** Stored anywhere on the host system.
    3.  **tmpfs:** Stored in the host's memory only; never written to the host's hard drive.


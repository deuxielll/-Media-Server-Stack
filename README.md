This repository provides a **production-ready Docker Compose setup** for a complete **media server stack**. It works on **Windows 10/11 with Docker Desktop** and includes apps for downloading, managing, and streaming your media.

---

## 🚀 Services Included

- **Jellyfin** → Free, open-source media streaming platform.
- **Jellyseerr** → Media request & discovery tool for Jellyfin.
- **Radarr** → Automated movie downloads (Usenet & BitTorrent).
- **Sonarr** → Automated TV show downloads (Usenet & BitTorrent).
- **Prowlarr** → Indexer manager & proxy for your *arr apps.
- **qBittorrent** → Powerful BitTorrent client with WebUI.
- **FlareSolverr** → Proxy to bypass Cloudflare protections.

---

## 📋 Prerequisites

Before running this stack, install:

- **Docker Desktop for Windows**
- **Docker Compose** → Already included with Docker Desktop.

⚠️ **Note for Windows users:**

- Run commands in **PowerShell or Command Prompt**, not Git Bash.
- Paths in `docker-compose.yml` use relative `./data/...` directories which will map inside the project folder (Windows-friendly).

---

## ⚙️ Setup Instructions

### 1. Clone the Repository

```powershell
git clone https://github.com/deuxielll/-Media-Server-Stack.git
cd -Media-Server-Stack

```

### 2. Copy and Configure Environment Variables

```powershell
copy .env.example .env
```

Then edit `.env` in **Notepad** or **VS Code**.

Key changes:

- `TZ=Asia/Manila` → Set your timezone (e.g., `America/New_York`).
- `PUID` / `PGID`:
    - On **Linux**, use `id -u` / `id -g`.
    - On **Windows**, leave them as `1000` (works fine with Docker Desktop).

### 3. Start the Stack

```powershell
docker compose up -d
```

This will:

- Download images
- Create containers
- Run everything in the background

---

## 🌐 Accessing Services

Once running, open your browser:

- **qBittorrent** → [http://localhost:8080](http://localhost:8080/)
- **Prowlarr** → [http://localhost:9696](http://localhost:9696/)
- **Jellyseerr** → [http://localhost:5055](http://localhost:5055/)
- **Jellyfin** → [http://localhost:8096](http://localhost:8096/)
- **Radarr** → [http://localhost:7878](http://localhost:7878/)
- **Sonarr** → [http://localhost:8989](http://localhost:8989/)
- **FlareSolverr** → [http://localhost:8191](http://localhost:8191/)

---

## 🛠 Common Docker Commands (Windows)

```powershell
# Stop all services
docker compose down

# View logs
docker compose logs -f

# Stop + remove containers, network, and volumes
docker compose down -v
```

---

## 📂 Notes & Windows Considerations

- **Volumes** → All configs, downloads, and libraries are stored in `.\data\` inside the project folder (works on Windows).
- **Restart Policy** → Containers auto-restart unless manually stopped.
- **Networking** → Uses a dedicated bridge `media-net`.
- **Permissions** → On Windows, you don’t need to change `PUID`/`PGID`.

---

## 🔧 Fixing qBittorrent "Unauthorized" Errors (Windows)

If you see **“Unauthorized”** when logging into qBittorrent:

1. **Stop the container**
    
    ```powershell
    docker compose stop qbittorrent
    
    ```
    
2. **Edit config file**
    - File: `.\data\config\qbittorrent\qBittorrent.conf`
    - Add under `[Preferences]`:
        
        ```
        WebUI\HostHeaderValidation=false
        WebUI\CSRFProtection=false
        ```
        
3. **Restart**
    
    ```powershell
    docker compose start qbittorrent
    ```
    
4. **Default login**
    - Username: `admin`
    - Password: Shown in container logs (first run only)
    
    ```powershell
    docker compose logs qbittorrent
    ```

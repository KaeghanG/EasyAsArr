# 🚀 EasyAsArr Docker Compose Setup

Welcome to the **EasyAsArr Docker Compose** setup! 🎉 This project simplifies the deployment of a media automation stack using Docker. It includes popular tools like **Sonarr, Radarr, Prowlarr, qBittorrent, Jellyfin,** and more, all routed through a **VPN** for enhanced privacy and security. 🔒

This setup uses **Gluetun** as a VPN client to ensure all traffic from the services is routed through a secure VPN connection. Below is a detailed explanation of the services and how they work together. 🛠️

## 📌 Table of Contents
- [⚡ Prerequisites](#-prerequisites)
- [🛠️ Services Overview](#-services-overview)
- [⚙️ Configuration](#-configuration)
- [▶️ Usage](#-usage)
- [🌍 Ports](#-ports)
- [💾 Volumes](#-volumes)
- [🔧 Environment Variables](#-environment-variables)
- [📜 License](#-license)

## ⚡ Prerequisites
Before you begin, ensure you have the following installed:

- [🐳 Docker](https://docs.docker.com/get-docker/)
- [📦 Docker Compose](https://docs.docker.com/compose/install/)
- A VPN subscription (e.g., **NordVPN**) with valid credentials.

## 🛠️ Services Overview

### 🔹 1. Gluetun (VPN Client)
- **Image:** `qmcgaw/gluetun`
- **Purpose:** Secures traffic from other containers through a **VPN connection**.
- **Configuration:**
  - Set your VPN provider (e.g., **NordVPN**) and credentials.
  - Choose VPN type (**OpenVPN** or **WireGuard**).
  - Specify the server country (e.g., **Netherlands**).

### 🔹 2. qBittorrent 🎥
- **Image:** `lscr.io/linuxserver/qbittorrent:latest`
- **Purpose:** A BitTorrent client for downloading media.
- **Configuration:**
  - Web UI accessible at port **8085**.
  - Default credentials: `admin/admin`.

### 🔹 3. Jackett 🔍
- **Image:** `lscr.io/linuxserver/jackett:latest`
- **Purpose:** Proxy server for torrent trackers, enabling integration with Sonarr and Radarr.

### 🔹 4. Sonarr 📺
- **Image:** `lscr.io/linuxserver/sonarr:latest`
- **Purpose:** Automates **TV show** downloads and organization.

### 🔹 5. Radarr 🎬
- **Image:** `lscr.io/linuxserver/radarr:latest`
- **Purpose:** Automates **movie** downloads and organization.

### 🔹 6. Prowlarr 📡
- **Image:** `lscr.io/linuxserver/prowlarr:latest`
- **Purpose:** Manages **indexers** for Sonarr and Radarr.

### 🔹 7. Jellyfin 🍿
- **Image:** `jellyfin/jellyfin`
- **Purpose:** A **media server** for streaming your downloaded content.

### 🔹 8. FileBrowser 📂
- **Image:** `hurlenko/filebrowser`
- **Purpose:** A **web-based file manager** for organizing your media files and providing an easy interface to locally download torrents from a UI.

### 🔹 9. FlareSolverr 🔄
- **Image:** `ghcr.io/flaresolverr/flaresolverr:latest`
- **Purpose:** A proxy server to bypass **CAPTCHAs** and restrictions on torrent sites.

### 🔹 10. Recyclarr 🔄
- **Image:** `ghcr.io/recyclarr/recyclarr:latest`
- **Purpose:** Automates the synchronization of **Trash Guide settings** with Sonarr and Radarr.

## ⚙️ Configuration

### 🛡️ 1. Gluetun VPN Setup
Edit the **environment** section under the Gluetun service to include your **VPN credentials**:
```yaml
environment:
  - VPN_SERVICE_PROVIDER=nordvpn
  - VPN_TYPE=openvpn # or wireguard
  - OPENVPN_USER=your_vpn_username
  - OPENVPN_PASSWORD=your_vpn_password
  - SERVER_COUNTRIES=Netherlands
```

### 📂 2. Volume Paths
Ensure **volume paths** in the `volumes` section match your local directory structure:
```yaml
volumes:
  - /home/ubuntu/docker/arr-stack/qbittorrent/config:/config
  - /home/ubuntu/docker/arr-stack/qbittorrent/downloads:/downloads
```

### 🔧 3. Environment Variables
Each service has **environment variables** for user IDs (`PUID`, `PGID`), timezone (`TZ`), and other settings. Adjust as needed.

## ▶️ Usage
1. **Clone this repository** or copy the `docker-compose.yml` file to your server.
2. **Update the configuration** as described above.
3. **Start the stack** using Docker Compose:
   ```bash
   docker-compose up -d
   ```
4. **Access services** via their respective **ports** (see [Ports](#-ports)).

## 🌍 Ports
| Service       | Port  | Purpose |
|--------------|------|---------|
| qBittorrent  | 8085 | 🎥 Web UI for qBittorrent |
| Jackett      | 9117 | 🔍 Web UI for Jackett |
| Sonarr       | 8989 | 📺 Web UI for Sonarr |
| Prowlarr     | 9696 | 📡 Web UI for Prowlarr |
| Radarr       | 7878 | 🎬 Web UI for Radarr |
| Jellyfin     | 8096 | 🍿 Web UI for Jellyfin |
| FileBrowser  | 1298 | 📂 Web UI for FileBrowser |
| FlareSolverr | 8191 | 🔄 API endpoint for FlareSolverr |

## 💾 Volumes
Ensure the following **directories exist** on your **host machine**:
```
/home/ubuntu/docker/arr-stack/gluetun
/home/ubuntu/docker/arr-stack/qbittorrent
/home/ubuntu/docker/arr-stack/jackett
/home/ubuntu/docker/arr-stack/sonarr
/home/ubuntu/docker/arr-stack/radarr
/home/ubuntu/docker/arr-stack/prowlarr
/home/ubuntu/docker/arr-stack/jellyfin
/home/ubuntu/docker/arr-stack/filebrowser
/home/ubuntu/docker/arr-stack/flaresolverr
/home/ubuntu/docker/arr-stack/recyclarr
```

## 🔧 Environment Variables
Key **environment variables**:
- `PUID`: User ID for file permissions.
- `PGID`: Group ID for file permissions.
- `TZ`: Timezone (e.g., `Europe/London`).
- `WEBUI_PORT`: Port for the service's web UI.
- `WEBUI_USERNAME` and `WEBUI_PASSWORD`: Web UI credentials.

## 📜 License
This project is **open-source** and available under the **MIT License**. Feel free to **modify** and **distribute** it as needed! 📝✨


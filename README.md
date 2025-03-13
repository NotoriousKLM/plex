Media Server Stack - Docker Compose Setup

This repository provides a Docker Compose configuration for a home media server stack. 
The stack includes Plex for media streaming, Traefik as a reverse proxy, and several supporting services for downloading and managing media (Gluetun, qbittorrent, sabnzbd, prowlarr, radarr, sonarr, and tautulli).

Note: All sensitive details (such as API keys and personal IP addresses) should be managed separately and not committed to version control.

Overview
Plex: Media server for streaming movies, TV shows, and other media. Configured with hardware transcoding support (using NVIDIA GPU if available).
Traefik: Reverse proxy handling incoming HTTP/HTTPS traffic.
Gluetun: VPN container for secure outbound connections.
qbittorrent: BitTorrent client for downloading content.
sabnzbd: NZB downloader for Usenet.
prowlarr: Indexer manager to support Radarr and Sonarr.
radarr: Movie management tool.
sonarr: TV series management tool.
tautulli: Monitoring tool for Plex activity.
Watchtower (optional): Automatically updates Docker containers on a defined schedule.
Prerequisites
Operating System: Ubuntu (latest stable build recommended).
Docker: Official Docker CE installed (not the Snap version).
Network: Gigabit internet over fiber, with the mesh router providing DHCP.
Hardware: A compatible Intel NIC and, if using hardware transcoding, an NVIDIA GPU with NVIDIA Container Toolkit installed.
Additional Tools: Ensure utilities like iperf3, ethtool, and sysctl are available for performance tuning and testing.
Setup Instructions
Clone the Repository:

bash
Copy
git clone <repository_url>
cd <repository_directory>
Review and Update the Docker Compose File:

Open docker-compose.yml and review the settings:

Volume Mappings:
Ensure the host paths (e.g., /media/plexmedia/Movies, /media/plexmedia/TV Shows, etc.) match your media directory structure.

Environment Variables:
Update values such as PUID, PGID, TZ, and any service-specific settings. Do not include any API keys or identifying IP addresses in this file.

Networking:
The configuration uses host networking for Plex and a custom Docker network for the other services.

Start the Services:

bash
Copy
sudo docker-compose up -d
Accessing the Services:

Plex:
Access the Plex Web UI at http://<server_ip>:32400/web
Traefik:
Traefik listens on ports 80 and 443. Ensure your DNS configuration points to your public IP.
Other Services:
Services like sabnzbd, radarr, sonarr, etc., are accessible on their published ports or via Traefik routing, as configured.
Testing Network Performance:

Use iperf3 to measure network throughput between devices.
Monitor NIC settings with ethtool and tune kernel TCP parameters using sysctl as needed.
Maintenance and Updates:

Automatic Updates:
If you use Watchtower, it can automatically update containers on a scheduled basis. Check its configuration for scheduling.

Logs and Monitoring:
Use sudo docker logs <container_name> to review logs.
Use tools like htop, iftop, and nload to monitor system performance.

Security & Network Configuration
DHCP and NAT:
The mesh router serves DHCP. The modem is configured in bridge mode (if applicable) to avoid double NAT issues.

Firewall:
Configure your firewall (e.g., UFW) to allow only the necessary ports for your services. Ensure SSH remains accessible.

Known Issues & Troubleshooting
Library Permissions:
Ensure that volume mounts have the correct ownership and permissions so that Plex (running as the non-root user defined by PUID/PGID) can access media files.

IPv6 Considerations:
If any services default to IPv6 (and you have it disabled), override with IPv4 settings as needed.

Networking Performance:
Test and tune the NIC ring buffers, TCP window sizes, and offloading settings for maximum throughput.

Disclaimer
This configuration is provided "as is" without any warranty. Please adapt and test it in your environment. No personal information is included, and all sensitive information (e.g., API keys) should be managed securely outside of this file.

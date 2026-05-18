# Hosting Guide For Durgaai Solutions Minecraft Stack

## Deployment Targets

- Minecraft Endpoint: `minecraft.durgaaisolutions.in:25565`
- Dashboard Endpoint: `https://minecraft.durgaaisolutions.in` (Password Protected)
- File Browser Endpoint: `http://minecraft.durgaaisolutions.in:8080`
- File Browser Username: `i8o8i`
- File Browser Password: `i8o8i`

## Prerequisites

- Ubuntu 22.04 Or 24.04 VPS
- Docker And Docker Compose Plugin Installed
- DNS `A` Record For `minecraft.durgaaisolutions.in`
- Open Firewall Ports:
  - `25565/tcp`
  - `24454/udp`
  - `8080/tcp`
  - `8123/tcp`
  - `4326/tcp`
  - `4327/tcp`
  - `80/tcp`
  - `443/tcp`

## Start Core Stack

```bash
docker compose pull
docker compose up -d
```

## Validate Services

```bash
docker compose ps
docker compose logs -f mc
```

## Host Frontend With Password Protection

### Install Nginx And Auth Utility

```bash
sudo apt update
sudo apt install -y nginx apache2-utils
```

### Create Dashboard Login

```bash
sudo htpasswd -c /etc/nginx/.dashboard_auth dashboard_admin
```

### Deploy Dashboard Files

```bash
sudo mkdir -p /var/www/minecraft-dashboard
sudo cp -r Dashboard/* /var/www/minecraft-dashboard/
sudo chown -R www-data:www-data /var/www/minecraft-dashboard
```

### Configure Nginx Site

Create `/etc/nginx/sites-available/minecraft-dashboard`:

```nginx
server {
  listen 80;
  server_name minecraft.durgaaisolutions.in;

  root /var/www/minecraft-dashboard;
  index index.html;

  auth_basic "Restricted";
  auth_basic_user_file /etc/nginx/.dashboard_auth;

  location / {
    try_files $uri $uri/ /index.html;
  }
}
```

Enable Site:

```bash
sudo ln -s /etc/nginx/sites-available/minecraft-dashboard /etc/nginx/sites-enabled/minecraft-dashboard
sudo nginx -t
sudo systemctl reload nginx
```

### Enable HTTPS

```bash
sudo apt install -y certbot python3-certbot-nginx
sudo certbot --nginx -d minecraft.durgaaisolutions.in
```

## Dashboard Data Behavior

`Dashboard/index.html` Uses:
- Live Minecraft Status API: `https://api.mcsrvstat.us/3/minecraft.durgaaisolutions.in`
- Optional Backend APIs:
  - `GET /api/players/recent`
  - `GET /api/plugins`
  - `GET /api/server-properties`
  - `POST /api/console/command`

If Optional Backend APIs Are Not Configured, The Dashboard Shows Professional Empty States Instead Of Dummy Data.

## Operational Commands

```bash
docker compose restart
docker compose pull
docker compose up -d
docker compose logs -f --tail=200 mc
docker compose logs -f --tail=200 rcon
docker compose logs -f --tail=200 filebrowser
```

## Security Recommendations

- Use Strong Secrets For `SERVICE_PASSWORD_RCON` And `RCON_WEB_PASS`
- Restrict Rcon Ports By Source IP If Possible
- Keep File Browser Behind Additional Reverse Proxy Auth If Exposed Publicly
- Back Up `MinecraftData` And `MinecraftBackups` Volumes Regularly

## Validation Checklist

- Minecraft Connects On `minecraft.durgaaisolutions.in:25565`
- Dashboard Login Prompt Appears On `https://minecraft.durgaaisolutions.in`
- Dashboard Overview Shows Live Server Data
- File Browser Accepts `i8o8i / i8o8i`
- Backup Volume Receives Scheduled Backups

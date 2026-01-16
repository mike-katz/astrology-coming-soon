# Ubuntu Server ma Deploy kari ne Commands

## Step 1: Server par connect karo
```bash
ssh username@your-server-ip
```
Example: `ssh root@192.168.1.100` or `ssh ubuntu@your-domain.com`

## Step 2: System update karo
```bash
sudo apt update
sudo apt upgrade -y
```

## Step 3: Nginx install karo (web server)
```bash
sudo apt install nginx -y
```

## Step 4: Nginx start karo
```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

## Step 5: Nginx status check karo
```bash
sudo systemctl status nginx
```

## Step 6: Web directory banavo
```bash
sudo mkdir -p /var/www/astroguruji
```

## Step 7: Directory permissions set karo
```bash
sudo chown -R $USER:$USER /var/www/astroguruji
sudo chmod -R 755 /var/www/astroguruji
```

## Step 8: Local machine thi files upload karo
### Option A: SCP use kari ne (local terminal ma run karo)
```bash
cd /Users/hardiksonagra/Projects/ReactJs/Astro/astro-coming
scp coming-soon.html username@your-server-ip:/var/www/astroguruji/
scp astro-guruji-logo.svg username@your-server-ip:/var/www/astroguruji/
scp astroguruji-fav-icon.svg username@your-server-ip:/var/www/astroguruji/
scp -r avatars username@your-server-ip:/var/www/astroguruji/
```

### Option B: Server par directly files create karo
Server par jai ne:
```bash
cd /var/www/astroguruji
nano coming-soon.html
```
(Paste your HTML code, then Ctrl+X, Y, Enter to save)

## Step 9: Nginx configuration file banavo
```bash
sudo nano /etc/nginx/sites-available/astroguruji
```

## Step 10: Nginx config ma aapde code paste karo:
```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    # Or IP address: server_name your-server-ip;

    root /var/www/astroguruji;
    index coming-soon.html;

    location / {
        try_files $uri $uri/ =404;
    }

    # SVG files support
    location ~* \.(svg)$ {
        add_header Content-Type image/svg+xml;
    }
}
```

Save karo: Ctrl+X, then Y, then Enter

## Step 11: Config file enable karo
```bash
sudo ln -s /etc/nginx/sites-available/astroguruji /etc/nginx/sites-enabled/
```

## Step 12: Default nginx config remove karo (optional)
```bash
sudo rm /etc/nginx/sites-enabled/default
```

## Step 13: Nginx config test karo
```bash
sudo nginx -t
```

## Step 14: Nginx restart karo
```bash
sudo systemctl restart nginx
```

## Step 15: Firewall allow karo (agar ufw use kari rahya ho to)
```bash
sudo ufw allow 'Nginx Full'
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw status
```

## Step 16: Browser ma check karo
Open browser and visit: `http://your-server-ip` or `http://your-domain.com`

---

## Quick Commands (Ek sath):

```bash
# Server par connect karo
ssh username@your-server-ip

# Update & Install
sudo apt update && sudo apt install nginx -y

# Directory banavo
sudo mkdir -p /var/www/astroguruji
sudo chown -R $USER:$USER /var/www/astroguruji
sudo chmod -R 755 /var/www/astroguruji

# Files upload karo (local machine thi)
# cd /Users/hardiksonagra/Projects/ReactJs/Astro/astro-coming
# scp coming-soon.html astro-guruji-logo.svg astroguruji-fav-icon.svg username@server-ip:/var/www/astroguruji/
# scp -r avatars username@server-ip:/var/www/astroguruji/

# Nginx config
sudo nano /etc/nginx/sites-available/astroguruji
# (Config paste karo)

# Enable & Restart
sudo ln -s /etc/nginx/sites-available/astroguruji /etc/nginx/sites-enabled/
sudo rm /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl restart nginx
```

---

## SSL Certificate (HTTPS) - Optional

### Certbot install karo
```bash
sudo apt install certbot python3-certbot-nginx -y
```

### SSL certificate generate karo
```bash
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```

### Auto-renewal check karo
```bash
sudo certbot renew --dry-run
```

---

## Troubleshooting

### Nginx error logs check karo
```bash
sudo tail -f /var/log/nginx/error.log
```

### Nginx access logs check karo
```bash
sudo tail -f /var/log/nginx/access.log
```

### Files permissions check karo
```bash
ls -la /var/www/astroguruji
```

### Nginx reload karo (config change pachhi)
```bash
sudo systemctl reload nginx
```

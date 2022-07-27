
# HOW INSTALL NGINX & SSL
ORIGINAL REPO [@bradtaversy](https://gist.github.com/bradtraversy/cd90d1ed3c462fe3bddd11bf8953a896#file-node_nginx_ssl-md)
## 1. Setup ufw firewall

```
sudo ufw enable
sudo ufw status
sudo ufw allow ssh (Port 22)
sudo ufw allow http (Port 80)
sudo ufw allow https (Port 443)
```

## 2. Install NGINX and configure

```
sudo apt install nginx

sudo nano /etc/nginx/sites-available/default
```

Add the following to the location part of the server block

```
    server_name yourdomain.com www.yourdomain.com;

    location / {
        proxy_pass http://localhost:5000; #whatever port your app runs on
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
```

```
# Check NGINX config
sudo nginx -t

# Restart NGINX
sudo service nginx restart
```

### You should now be able to visit your IP with no port (port 80) and see your app. Now let's add a domain
3. Add SSL with LetsEncrypt

```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update

FOR UBUNTU GREATER THAN 20
sudo apt-get install python3-certbot-nginx

FOR EARLIER VERSIONS
sudo apt-get install python-certbot-nginx

sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run
```

Now visit https://yourdomain.com and you should see your Node app

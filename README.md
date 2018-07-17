# io-game
Learning IO game frameworks.

## Ubuntu Server Setup

### Certbot

```
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install python-certbot-nginx
```

### Nginx

Use Nginx as a reverse proxy with signed SSL cert.

```
$ sudo apt-get update
$ sudo apt-get install nginx
```

Check the status of nginx

```
$ systemctl status nginx
```

```
Output
● nginx.service - A high performance web server and a reverse proxy server
  Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
  Active: active (running) since Mon 2016-04-18 16:14:00 EDT; 4min 2s ago
  Main PID: 12857 (nginx)
  CGroup: /system.slice/nginx.service
	  ├─12857 nginx: master process /usr/sbin/nginx -g daemon on; master_process on
	  └─12858 nginx: worker process
```

Commands

```
$ sudo systemctl stop nginx
$ sudo systemctl start nginx
$ sudo systemctl restart nginx
```

Change the domains

```
sudo emacs /etc/nginx/sites-available/default
```

Replace `_` after `server_name`

```
server_name example.com www.example.com;
```

Replace `location /` block.

```
location / {
	 proxy_pass http://localhost:8080;
	 proxy_http_version 1.1;
	 proxy_set_header Upgrade $http_upgrade;
	 proxy_set_header Connection 'upgrade';
	 proxy_set_header Host $host;
	 proxy_cache_bypass $http_upgrade;
}
```

Check the config

```
$ sudo nginx -t
```

Reload

```
$ sudo systemctl reload nginx
```

### Firewall

Server uses `ufw` firewall by default.

```
$ sudo ufw allow ssh
$ sudo ufw enable
```

`ufw` opens port 20 for Nginx if it's detected.

```
& sudo ufw allow 'Nginx Full'
& sudo ufw delete `Nginx HTTP'
```

```
$ sudo ufw status
```

```

Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx Full                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx Full (v6)            ALLOW       Anywhere (v6)
```

### Obtain SSL Certificates

```
$ sudo certbot --nginx -d example.com -d www.example.com
```

Verify that the renewal process is working.

```
$ sudo certbot renew --dry-run
```

### Manage Application with PM2

PM2 is an application manager that works well with NodeJS.

```
$ sudo npm install -g pm2
```

```
$ pm2 start hello.js
```

Running the pm2 startup command outputs a script that causes pm2 to run at startup.

```
$ pm2 startup systemd
```

List the running servers.

```
$ pm2 list
```

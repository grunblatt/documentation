install nginx
```bash
sudo apt update
sudo apt install nginx
```
adjust the firewall
```bash
sudo ufw app list
```
output
```
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
  ```
  ```bash
  sudo ufw allow 'Nginx HTTP'
  sudo ufw allow 'ftp'
  sudo ufw allow 'OpenSSH'
  ufw --force enable
  ```
  ```bash
  sudo ufw status
  ```
  output
  ```
  Status: active

To                         Action      From
--                         ------      ----
21/tcp                     ALLOW       Anywhere                
OpenSSH                    ALLOW       Anywhere     
Nginx HTTP                 ALLOW       Anywhere
21/tcp (v6)                ALLOW       Anywhere (v6)             
OpenSSH (v6)               ALLOW       Anywhere (v6)  
Nginx HTTP(v6)             ALLOW       Anywhere(v6)

```
check the web server
```bash
systemctl status nginx
```
output
```bash
● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2020-04-20 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   Memory: 3.5M
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```
enter your ip address in the browser and you should got this
```
Welcome to nginx!
If you see this page, the nginx web server is successfully installed and working. Further configuration is required.

For online documentation and support please refer to nginx.org.
Commercial support is available at nginx.com.

Thank you for using nginx.
```
```bash
sudo nano /etc/nginx/sites-available/<your_domain>
```
enter in the file 
```c
server {

        root /var/www/<your_domain>/html;
        index index.html index.htm index.nginx-debian.html;

        server_name<your_domain>;
    

     location / {
                proxy_pass http://localhost:<your_port>;
	proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection $http_connection;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
        }



}

```
```bash
sudo ln -s /etc/nginx/sites-available/<your_domain> /etc/nginx/sites-enabled/
```
```bash
sudo nano /etc/nginx/nginx.conf
```
find the server_names_hash_bucket_size directive and remove the # symbol to uncomment the line
```c
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
```
test 
```bash
sudo nginx -t
```
restart
```bash
sudo systemctl restart nginx
```
---
make sure to have a domain name.
    
install certbot
```bash
sudo apt install certbot python3-certbot-nginx
```
```bash
sudo nginx -t
```
```bash
sudo systemctl reload nginx
```
```bash
sudo ufw status
```
output
```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere                  
Nginx HTTP                 ALLOW       Anywhere                  
OpenSSH (v6)               ALLOW       Anywhere (v6)             
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```
```bash
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```
```bash
sudo certbot --nginx -d example.com 
```
output
```
Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```
select your choice then hit ENTER
output
```
IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on 2020-08-18. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
   ```
   ```bash
   sudo systemctl status certbot.timer
   ```
   output
   ```
   ● certbot.timer - Run certbot twice daily
     Loaded: loaded (/lib/systemd/system/certbot.timer; enabled; vendor preset: enabled)
     Active: active (waiting) since Mon 2020-05-04 20:04:36 UTC; 2 weeks 1 days ago
    Trigger: Thu 2020-05-21 05:22:32 UTC; 9h left
   Triggers: ● certbot.service
   ```

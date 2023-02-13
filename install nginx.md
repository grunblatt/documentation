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
Nginx Full                 ALLOW       Anywhere                  
OpenSSH                    ALLOW       Anywhere                  
21/tcp (v6)                ALLOW       Anywhere (v6)             
Nginx Full (v6)            ALLOW       Anywhere (v6)             
OpenSSH (v6)               ALLOW       Anywhere (v6)  
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
sudo nano /etc/nginx/sites-available/your_domain
```
enter in the file 
```c
server {

        root /var/www/devapi.srx365.sprxdev.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name devapi.srx365.sprxdev.com;
    

     location / {
                proxy_pass https://localhost:7130;
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
sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
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
  ---
   Installing .NET Core Runtime
   ```bash
   sudo add-apt-repository universe
   ```
   ```bash
   sudo apt install apt-transport-https
   ```
   ```bash
   sudo apt update
   ```
   ```bash
   sudo apt install dotnet-sdk-6.0
   ```
   ```bash
   sudo mkdir -p /var/www/<project_name>
   ```
   ```bash
   cd /var/www/<project_name>
   git clone <your_github_url>
   ```
   ```bash
   dotnet build
   ```
   ```bash
   dotnet publish
   ```
   ```bash
   sudo nginx -s reload
   ```
   ```bash
   cd /etc/systemd/system
   ```
   ```bash
   sudo nano <your_project_name>.service
   ```
   add the following content to it (the following is an example, change all the project related content)
   ```c
   [Unit]
Description=SRX365_API

[Service]
WorkingDirectory=/var/www/<project_name>
ExecStart=/usr/bin/dotnet /var/www/<project_path>/bin/Debug/net6.0/publish/<project_name>.dll
Restart=always
RestartSec=10
SyslogIdentifier=<project_nmae>
User=root
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl enable <your_project_name>.service
```
```bash
sudo systemctl start movie.service
```
```bash
sudo systemctl status movie.service
```
(this may take some time)  
Output  
(somthing like that)
```
movie.service - Movie app
   Loaded: loaded (/etc/systemd/system/movie.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2019-06-23 04:51:28 UTC; 11s ago
 Main PID: 6038 (dotnet)
    Tasks: 16 (limit: 1152)
   CGroup: /system.slice/movie.service
           └─6038 /usr/bin/dotnet /var/www/movie-app/bin/Debug/netcoreapp2.2/publish/MvcMovie.dll
```           
---
---
first create a new droplet for your db,
do the following in the new droplet

setting up a remote postgres database server

```bash
sudo apt-get update
```
```bash
sudo apt-get install postgresql postgresql-contrib
```
```bash
createuser --interactive --pwprompt 
```
a prompt will be shown to you asking you to input your desired user role, name, password
```bash
Enter name of role to add: <enter_a_name>
Enter password for new role:
Enter it again:
Shall the new role be a superuser? (y/n) y
```
```bash
createdb -O <your_name> <db_name>
```
```bash
nano /etc/postgresql/<your_version>/main/postgresql.conf
```
look for this line in the file
```
#listen_addresses = 'localhost'
```
uncomment, and change the value to '*'
```
listen_addresses = '*'
```
```bash
nano /etc/postgresql/10/main/pg_hba.conf
```
mdify this section
```
# IPv4 local connections:
host    all             all             127.0.0.1/32            md5
```
to this
```
# IPv4 local connections:
host    all             all             0.0.0.0/0            md5
```
```
sudo ufw allow 5432/tcp
```
```
sudo systemctl restart postgresql
```
```
dotnet ef migrations add initial
```
```
dotnet ef database update
```
---
---
go back to the back end droplet

Install pm2
```
apt install npm
```
```
npm install pm2 -g
```
```
apt update && apt install sudo curl && curl -sL https://raw.githubusercontent.com/Unitech/pm2/master/packager/setup.deb.sh | sudo -E bash -
```
create a file in your project folder
```
ecosystem.config.js
```
enter
```
module.exports = {
    apps : [{
      name: "/var/www/<project_path>",
      script: "dotnet",
      args: "run",
      cwd:"/var/www/<project_path>",
      max_memory_restart: "200M",
      max_restarts: 200,  exp_backoff_restart_delay: 1000,    
    }
  ]
  }
  ```

```
pm2 start var/www<your_path_ecosystem.config.js>
```
to view status
```
pm2 logs
````
---

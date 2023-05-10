```
cd ~
curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
```

```
sudo bash nodesource_setup.sh
```

```
sudo apt install nodejs
```
```
node -v
```
```
apt install npm
```
```
npm -v
```
```
git clone <your project url>
```
enter your username amd Personal access tokens
```
cd <your project>
```
```
npm install
```
make sure you have the .env file

```
sudo npm install pm2@latest -g
```
```
pm2 start <your project starting point (server.js)>
```
you shoul see somthing like this
```
Output

[PM2] Spawning PM2 daemon with pm2_home=/home/sammy/.pm2
[PM2] PM2 Successfully daemonized
[PM2] Starting /home/sammy/hello.js in fork_mode (1 instance)
[PM2] Done.
┌────┬────────────────────┬──────────┬──────┬───────────┬──────────┬──────────┐
│ id │ name               │ mode     │ ↺    │ status    │ cpu      │ memory   │
├────┼────────────────────┼──────────┼──────┼───────────┼──────────┼──────────┤
│ 0  │ hello              │ fork     │ 0    │ online    │ 0%       │ 25.2mb   │
└────┴────────────────────┴──────────┴──────┴───────────┴──────────┴──────────┘
```
```
pm2 startup systemd
```
you shoul see somthing like this
```
Output
[PM2] Init System found: systemd
root
[PM2] To setup the Startup Script, copy/paste the following command:
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u root --hp /home/root
```
```
sudo systemctl start pm2-root
```
```
systemctl status pm2-root
```
```
sudo apt install nginx
```
enter in /etc/nginx/sites-enabled/default
```
  proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
```
```
nginx -t
```
```
sudo systemctl restart nginx
```
```
sudo systemctl restart pm2-root
```
```
pm2 start  server.js
```


now your ip address should work
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
```
su - postgres
```
```
psql
```
You will be shown something similar to this:
```
postgres@logrocket:~$ psql
psql (10.12 (Ubuntu 10.12-0ubuntu0.18.04.1))
Type "help" for help
postgres=#
```
```
\q
```

```bash
createuser --interactive --pwprompt 
```
a prompt will be shown to you asking you to input your desired user role, name, password
```bash
Enter name of role to add: <enter root>
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
host    all             all             <ip adderss of the backend droplet>/32            md5
```
```
su - root
```
```
sudo ufw allow 5432/tcp
```
```
sudo systemctl restart postgresql
```
---
---
after having set up a database make sure the connection string is correct
```
pm2 restart <your project starting point (server.js)> (or start not restart)
```
to build and migrate the data to the database
```
npx prisma migrate dev --name init
```
after pointing a domain name to the ip address
```
sudo apt install certbot python3-certbot-nginx
```
```
sudo nginx -t
```
```
sudo systemctl reload nginx
```
```
sudo ufw allow 'Nginx Full'
sudo ufw delete allow 'Nginx HTTP'
```
```
sudo certbot --nginx -d example.com 
```
```
sudo systemctl status certbot.timer
```

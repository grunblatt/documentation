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
cd /var
```
```
mkdir www
```
```
cd www
```
```
mkdir <your domain name>
```
```
cd <your domain name>
```
```
git clone <your project url>
```
enter your username and Personal access tokens
```
cd <your project>
```
```
make sure you have the .env file
```
```
npm install
```
```
sudo npm install pm2@latest -g
```
```
pm2 start <your project starting point (server.js)>
```
you should see something like this.
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
you should see something like this
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
----
if you need a DB - you can do this on the same droplet or on a new one.
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
host    all             all            0.0.0.0/0            md5
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

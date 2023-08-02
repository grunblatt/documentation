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
you shoul see something like this
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

fallow the [it_documentation](https://github.com/CloudSnob/it_documentation/blob/main/nginx/static_website_hosting.md)

```
export domain=<your domain> 
export customer=<cutomer name> #camel case
sudo mkdir -p /var/www/${customer}/${domain}
```
```
sudo echo -en "server {\n    root /var/www/$customer/$domain;\n    index index.html index.htm index.nginx-debian.html;\n    server_name $domain;\n\n    location / {\n        try_files \$uri /index.html;\n    }\n}" > /etc/nginx/sites-available/$domain
ln -s  /etc/nginx/sites-available/$domain /etc/nginx/sites-enabled/$domain
```
```
sudo certbot --nginx -d ${domain}
```
If asked, choose to Redirect to https
```
sudo systemctl restart nginx
```
in git bash run
```
npm run <the file with the env variable's> (lookup in package.json)
```
copy the files you build to your domain folder in the static-html droplet
# Deploy Reactjs & Laravel with subdirectory using NGINX

# Overview
This what you will need to know if you are building a react-app on top of (ASP.NET Web API) any project on one single domain. So the API will sit at the root and the UI in a sub-folder in order to access Active Directory credentials for an Intranet application.

# React App in subdirectory

First step is to update the **package.json** file from the root of your application.
Add an entry for **"homepage"** in the same section with ***“name”** towards the top of the JSON file.
```
"homepage":"/Project-1"
```
- before that running **npm run build** The URL should be the domain plus the sub-folder.
- Add this code to your Nginx configuration file, typically found at **/etc/nginx/sites-available/project-1-frontend**, at the bottom of the server block:
- Don`t forget to restart Nginx.
```
sudo systemctl restart nginx
```
- you can check the full configration on github repo file name **" "**.


**Nginx config:**
```
    location =  /Project-1 {

            root /var/www/myapp-frontend/build;
            try_files $uri $uri/ /Project-1/index.html;
    }

    location ~ ^/Project-1(.*) {

            root /var/www/myapp/build;
            try_files $1 $1/ /index.html =404;
    }
```

# Laravel App in subdirectory

- What we try to achieve here is to run the Laravel application in a subdirectory of the main domain. For example: **domain.com/Project-1/**
- For the matter of semantics and good practice, we will place our second Laravel install in the folder: **/var/www/project-1-backend**
- Add this code to your Nginx configuration file, typically found at **/etc/nginx/sites-available/project-1-backend**, at the bottom of the server block:
- you can check the full configration on github repo file name **" "**.

**Nginx config:**
```
location /project-1 {
    alias /var/www/myapp-backend/public;
    try_files $uri $uri/ @project-1;

    location ~ \.php$ {
        #include snippets/fastcgi-php.conf;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $request_filename;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
    }
}


location @project-1 {
    rewrite /project-1/(.*)$ /project-1/index.php?/$1 last;
}
```

**Please note the version number for php-fpm in the fastcgi_pass directive.**
- If you are using a different version of php-fpm you will need to adjust the version number here, otherwise you will get a **502 Bad Gateway** error from Nginx.
- We introduced here an **alias** directive for the path instead of **root** inside location block. 
- Don`t forget to restart Nginx.
```
sudo systemctl restart nginx
```
- you can check the full configration on github repo file name **" "**.


**hope you will enjoyed it.**



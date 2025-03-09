# CSlant gitlab project

This project is a simple project to set up a gitlab server with docker-compose.

## How to use

1. Clone the repository

```bash
git clone https://gitlab.com/cslant/gitlab.git
```

2. Change to the repository directory

```bash
cd gitlab
```

3. Create the environment file

```bash
cp .env.example .env
```

4. Edit the environment file

```bash
nano .env
```

5. Start the gitlab server

```bash
docker-compose up -d
```

6. Setup reverse proxy

You can use the following nginx configuration to setup a reverse proxy for the gitlab server.

```nginx
server_tokens off;
root /another/path;

add_header Strict-Transport-Security "max-age=63072000" always;
add_header X-Frame-Options "SAMEORIGIN";
add_header X-XSS-Protection "1; mode=block";
add_header X-Content-Type-Options "nosniff";

index index.html index.php;

charset utf-8;

client_max_body_size 1024m;

location / {
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_cache_bypass $http_upgrade;
    # # proxy_read_timeout 86400s;
    # # proxy_send_timeout 86400s;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_pass http://127.0.0.1:802;
    proxy_read_timeout 300;
    proxy_connect_timeout 300;
    proxy_redirect off;
    
    proxy_set_header X-Forwarded-Proto https;
    proxy_set_header X-Frame-Options SAMEORIGIN;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Ssl on;
}

location = /favicon.ico { access_log off; log_not_found off; }
location = /robots.txt  { access_log off; log_not_found off; }

access_log off;
error_log  /var/log/nginx/gitlab.mydomain.io-error.log error;

error_page 404 /index.php;

location ~ \.php$ {
    try_files $uri $uri/ =404;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass unix:/var/run/php/php8.4-fpm.sock;
    fastcgi_index index.php;
    include fastcgi_params;
}

# Deny files starting with a . (dot) except .well-known
location ~ /\.(?!well-known).* {
    deny all;
}
```

7. Access the gitlab server

Open your browser and go to `https://gitlab.mydomain.io`

# nginx部署

## centos/linux部署
* yum install nginx -y
* systemctl start nginx.service
* systemctl enable nginx.service


## 配置xxx.conf
* cd /etc/nginx/conf.d
* vi xxx.conf
```
# http
server {
    listen          80;
    server_name     xxx.example.com;
    client_max_body_size 100M;
    index    index.shtml index.php index.html index.htm index.jsp;
    charset UTF-8;

    location / {
       root /opt/webapps/example;
       try_files $uri $uri/ /index.html;
    }

    #代理
    location /api {
        proxy_pass http://127.0.0.1:8080/;
        proxy_set_header  Host $http_host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Real-Port $remote_port;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    #images资源文件
    location /images {
        root /opt/webapps/images;
        rewrite ^/images/(.*)$ /$1 break;
        expires 30d;
    }
    #指定某个文件
    location /useragreement {
         root  /opt/webapps;
         try_files $uri $uri/ /useragreement.html;
    }
}

# https
server {
    listen          80;
    server_name     xxx.example.com;
    charset UTF-8;
    rewrite ^/(.*) https://$server_name$1 permanent;
}
server {
    listen          443 ssl;
    server_name     xxx.example.com;
    client_max_body_size 100M;
    index    index.shtml index.php index.html index.htm index.jsp;
    charset UTF-8;

    location / {
       root /opt/webapps/example;
       try_files $uri $uri/ /index.html;
    }

    #代理
    location /api {
        proxy_pass http://127.0.0.1:8080/;
        proxy_set_header  Host $http_host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Real-Port $remote_port;
        proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    #images资源文件
    location /images {
        root /opt/webapps/images;
        rewrite ^/images/(.*)$ /$1 break;
        expires 30d;
    }
    #指定某个文件
    location /useragreement {
         root  /opt/webapps;
         try_files $uri $uri/ /useragreement.html;
    }
    
    ssl_certificate /etc/nginx/ssl/xxx.example.com.pem;
    ssl_certificate_key /etc/nginx/ssl/xxx.example.com.key;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
}

# websocket
server {
    listen          80;
    server_name     xxx.example.com;
    location / {
        proxy_pass http://127.0.0.1:8080/;
        proxy_connect_timeout 4s;
        proxy_read_timeout 7200s;
        proxy_send_timeout 12s;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```
* systemctl restart nginx.service


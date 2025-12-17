Nginx.conf配置文件位置

/usr/local/nginx/conf/nginx.conf

Nginx SSL证书文件位置

/usr/local/webserver/nginx/ssl

Nginx 配置文件如下：

#user nobody;

worker_processes  1;

#error_log  logs/error.log;

#error_log  logs/error.log  notice;

#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {

    worker_connections  1024;

}

# siceng

stream {

upstream wms1 {

server 172.16.1.37:14530;

}

upstream wms2 {

                server 172.16.1.37:14531;

        }

upstream wms3 {

                server 172.16.1.37:14630;

        }

upstream wms4 {

                server 172.16.1.37:14631;

        }

upstream ld1 {

                server 192.168.1.38:20080;

        }

upstream ld2 {

                server 192.168.1.38:20081;

        }

upstream ld3 {

                server 192.168.1.38:20082;

        }

upstream ld4 {

                server 192.168.1.38:20083;

        }

upstream erp {

                server 192.168.1.39:8004;

        }

upstream oa8088 {

                server 10.0.2.15:8088;

        }

upstream oa89 {

                server 10.0.2.15:89;

        }

upstream oa5222 {

                server 10.0.2.15:5222;

        }

upstream oa7070 {

                server 10.0.2.15:7070;

        }

upstream oa9090 {

                server 10.0.2.15:9090;

        }

upstream jili8000 {

                server 10.0.17.102:8000;

        }

upstream jili8001 {

                server 10.0.17.102:8001;

        }

upstream jili8002 {

                server 10.0.17.102:8002;

        }

#wms1

server {

listen 14530;

proxy_pass wms1;

}

#wms2

server  {

         listen 14531;

         proxy_pass wms2;

         }

#wms3

server {

                listen 14630;

                proxy_pass wms3;

        }

#wms4

server  {

         listen 14631;

         proxy_pass wms4;

         }

# ld1

server {

                listen 20080;

                proxy_pass ld1;

        }

# ld2

server {

                listen 20081;

                proxy_pass ld2;

        }

# ld3

server {

                listen 20082;

                proxy_pass ld3;

        }

# ld4

server {

                listen 20083;

                proxy_pass ld4;

        }

# erp

        server {

                listen 8004;

                proxy_pass erp;

        }

#OA8088

        server {

                listen 8088;

                proxy_pass oa8088;

        }

#OA89

        server {

                listen 89;

                proxy_pass oa89;

        }

#OA5222

        server {

                listen 5222;

                proxy_pass oa5222;

        }

#OA7070

        server {

                listen 7070;

                proxy_pass oa7070;

        }

#OA9090

        server {

                listen 9090;

                proxy_pass oa9090;

        }

#jili8000

        server {

                listen 8000;

                proxy_pass jili8000;

        }

#jili8001

        server {

                listen 8001;

                proxy_pass jili8001;

        }

#jili8002

        server {

                listen 8002;

                proxy_pass jili8002;

        }

}

http {

    include       mime.types;

    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '

    #                  '$status $body_bytes_sent "$http_referer" '

    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;

    #tcp_nopush     on;

    #keepalive_timeout  0;

    keepalive_timeout  65;

    #gzip  on;

    server {

        listen       80;

        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {

            root   html;

            index  index.html index.htm;

        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html

        #

        error_page   500 502 503 504  /50x.html;

        location = /50x.html {

            root   html;

        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80

        #

        #location ~ \.php$ {

        #    proxy_pass   http://127.0.0.1;

        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000

        #

        #location ~ \.php$ {

        #    root           html;

        #    fastcgi_pass   127.0.0.1:9000;

        #    fastcgi_index  index.php;

        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;

        #    include        fastcgi_params;

        #}

        # deny access to .htaccess files, if Apache's document root

        # concurs with nginx's one

        #

        #location ~ /\.ht {

        #    deny  all;

        #}

    }

upstream zabbix{

        server 10.0.2.22:80;

}

upstream grafana{

        server 10.0.2.23:3000;

}

        upstream oa{

                server 10.0.2.15:8011;

        }

        upstream oa8999{

                server 10.0.2.15:8999;

        }

        upstream huorong{

                server 192.168.1.240:8089;

        }

        upstream adm{

                server 10.0.2.17:2000;

        }

        upstream jump{

                server 10.0.2.30:5000;

        }

# Zabbix

   server {

listen 80;

server_name zabbix.songyuansafety.com;

location / {

# rewrite (.*) https://zabbix.songyuansafety.com$1;

rewrite ^(.*) https://$server_name$request_uri permanent;

}

}

server {

         listen       443 ssl;

        server_name  zabbix.songyuansafety.com;

#         ssl_certificate      /usr/local/webserver/nginx/ssl/OA.crt;

                ssl_certificate      /usr/local/webserver/nginx/ssl/zabbix.songyuansafety.com.cer;

         ssl_certificate_key  /usr/local/webserver/nginx/ssl/zabbix.songyuansafety.com.key;

         ssl_session_cache    shared:SSL:1m;

         ssl_session_timeout  5m;

         ssl_ciphers  HIGH:!aNULL:!MD5;

         ssl_prefer_server_ciphers  on;

         location / {

             proxy_pass http://zabbix;

         }

}

#Grafana

server {

        listen 80;

        server_name grafana.songyuansafety.com;

        location / {

#               rewrite (.*) https://grafana.songyuansafety.com$1;

rewrite ^(.*) https://$server_name$request_uri permanent;

        }

        }

  server {

        listen       443 ssl;

        server_name  grafana.songyuansafety.com;

        ssl_certificate      /usr/local/webserver/nginx/ssl/grafana.songyuansafety.com.cer;

        ssl_certificate_key  /usr/local/webserver/nginx/ssl/grafana.songyuansafety.com.key;

        ssl_session_cache    shared:SSL:1m;

        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers  on;

        location / {

  proxy_set_header Host grafana.songyuansafety.com;

 add_header 'Access-Control-Allow-Origin' '*';

                add_header Access-Control-Allow-Methods GET,POST,OPTIONS,DELETE;

                add_header 'Access-Control-Allow-Headers' 'userId,DNT,X-CustomHeader,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';

            proxy_pass http://grafana;

        }

}

#OA

server {

        listen 80;

        server_name oa.songyuansafety.com;

        location / {

#               rewrite (.*) https://oa.songyuansafety.com$1;

rewrite ^(.*) https://$server_name$request_uri permanent;

        }

        }

  server {

        listen       443 ssl;

        server_name  oa.songyuansafety.com;

        ssl_certificate      /usr/local/webserver/nginx/ssl/oa.songyuansafety.com.cer;

        ssl_certificate_key  /usr/local/webserver/nginx/ssl/oa.songyuansafety.com.key;

        ssl_session_cache    shared:SSL:1m;

        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers  on;

        location / {

            proxy_pass http://oa;

            client_max_body_size 100m;

            }

}

#OA

server {

        listen 80;

        server_name oa8999.songyuansafety.com;

        location / {

#               rewrite (.*) https://oa8999.songyuansafety.com$1;

rewrite ^(.*) https://$server_name$request_uri permanent;

        }

        }

  server {

        listen       443 ssl;

        server_name  oa8999.songyuansafety.com;

        ssl_certificate      /usr/local/webserver/nginx/ssl/oa8999.songyuansafety.com.cer;

        ssl_certificate_key  /usr/local/webserver/nginx/ssl/oa8999.songyuansafety.com.key;

        ssl_session_cache    shared:SSL:1m;

        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers  on;

        location / {

            proxy_pass http://oa8999;

        }

}

#huorong

server {

        listen 80;

        server_name huorong.songyuansafety.com;

        location / {

#               rewrite (.*) https://huorong.songyuansafety.com$1;

rewrite ^(.*) https://$server_name$request_uri permanent;

        }

        }

  server {

        listen       443 ssl;

        server_name  huorong.songyuansafety.com;

        ssl_certificate      /usr/local/webserver/nginx/ssl/huorong.songyuansafety.com.cer;

        ssl_certificate_key  /usr/local/webserver/nginx/ssl/huorong.songyuansafety.com.key;

        ssl_session_cache    shared:SSL:1m;

        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers  on;

        location / {

            proxy_pass http://huorong;

        }

}

#adm_plus

server {

        listen 80;

        server_name adm.songyuansafety.com;

        location / {

#               rewrite (.*) https://adm.songyuansafety.com$1;

rewrite ^(.*) https://$server_name$request_uri permanent;

        }

        }

  server {

        listen       443 ssl;

        server_name  adm.songyuansafety.com;

        ssl_certificate      /usr/local/webserver/nginx/ssl/huorong.songyuansafety.com.cer;

        ssl_certificate_key  /usr/local/webserver/nginx/ssl/huorong.songyuansafety.com.key;

        ssl_session_cache    shared:SSL:1m;

        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers  on;

        location / {

            proxy_pass http://adm;

        }

}

#jumpserver

server {

        listen 80;

        server_name jump.songyuansafety.com;

        location / {

rewrite ^(.*) https://$server_name$request_uri permanent;

        }

        }

  server {

        listen       443 ssl;

        server_name  jump.songyuansafety.com;

    proxy_request_buffering off;  

    proxy_http_version 1.1;       

    proxy_set_header Host $host;  

    proxy_set_header Upgrade $http_upgrade;

    proxy_set_header Connection $http_connection;

    proxy_set_header X-Forwarded-For $remote_addr;

    proxy_ignore_client_abort on;                 

    proxy_connect_timeout 600;                    

    proxy_send_timeout 600;                       

    proxy_read_timeout 600;                       

    send_timeout 6000;                            

        ssl_certificate      /usr/local/webserver/nginx/ssl/jump.songyuansafety.com.cer;

        ssl_certificate_key  /usr/local/webserver/nginx/ssl/jump.songyuansafety.com.key;

        ssl_session_cache    shared:SSL:1m;

        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers  on;

        location / {

            proxy_pass http://jump;

        }

}

}
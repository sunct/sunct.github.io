---
title: ' 当前nginx 主配置信息（备用）'
tags:
  - nginx
  - 配置
id: '545'
categories:
  - - 服务
date: 2019-04-22 15:50:52
---

nginx 主配置，其他子站配置在路径 /www/server/panel/vhost/nginx/下 ，这样更方便于管理个个二级域名。

user  www www;  
worker\_processes auto;  
error\_log  /www/wwwlogs/nginx\_error.log  warn;  
pid        /www/server/nginx/logs/nginx.pid;  
worker\_rlimit\_nofile 51200;

events  
{  
use epoll;  
worker\_connections 51200;  
multi\_accept on;  
}

http  
{  
include mime.types;  
include proxy.conf;

```
    default_type  application/octet-stream;

    server_names_hash_bucket_size 512;
    client_header_buffer_size 32k;
    large_client_header_buffers 4 32k;
    client_max_body_size 10m;

    sendfile   on;
    tcp_nopush on;

    keepalive_timeout 60;

    tcp_nodelay on;

    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 256k;
    fastcgi_intercept_errors on;

    gzip on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.1;
    gzip_comp_level 6;
    gzip_types     text/plain application/javascript application/x-javascript text/javascript text/css application/xml;
    gzip_vary on;
    gzip_proxied   expired no-cache no-store private auth;
    gzip_disable   "MSIE [1-6]\.";

    limit_conn_zone $binary_remote_addr zone=perip:10m;
    limit_conn_zone $server_name zone=perserver:10m;

    server_tokens off;
    access_log off;
```

server  
{  
listen 1236;

```
       server_name www.bt.cn;
    index index.html index.htm index.php;
    root  /www/server/phpmyadmin;

    #error_page   404   /404.html;
#AUTH_START
auth_basic "Authorization";
auth_basic_user_file /www/server/pass/phpmyadmin.pass;
#AUTH_END
    include enable-php.conf;

    location ~ .*\.(gifjpgjpegpngbmpswf)$
    {
        expires      30d;
    }

    location ~ .*\.(jscss)?$
    {
        expires      12h;
    }

    location ~ /\.
    {

    }


    access_log  /www/wwwlogs/access.log;
}
```

include /www/server/panel/vhost/nginx/\*.conf;  
}

* * *

主站配置

server  
{  
    listen 80;  
    listen 443 ssl;  
    server\_name www.sunsanmiao.cn;  
    index index.php index.html index.htm default.php default.htm default.html;  
    root /www/wwwroot/wordpress;

```
#SSL-START SSL相关配置，请勿删除或修改下一行带注释的404规则
#error_page 404/404.html;
#HTTP_TO_HTTPS_START
if ($server_port !~ 443){
    rewrite ^(/.*)$ https://$host$1 permanent;
}
#HTTP_TO_HTTPS_END
limit_conn perserver 300;
limit_conn perip 25;
limit_rate 512k;
ssl_certificate    /etc/letsencrypt/live/www.sunsanmiao.cn/fullchain.pem;
ssl_certificate_key    /etc/letsencrypt/live/www.sunsanmiao.cn/privkey.pem;
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
ssl_prefer_server_ciphers on;
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 10m;
error_page 497  https://$host$request_uri;






#SSL-END

#ERROR-PAGE-START  错误页配置，可以注释、删除或修改
#error_page 404  https://www.sunsanmiao.cn/404-2.html
#error_page 502 /502.html;
#ERROR-PAGE-END

#PHP-INFO-START  PHP引用配置，可以注释或修改
#SECURITY-START 防盗链配置
location ~ .*\.(jpgjpeggifpngjscss)$
{
    expires      30d;
    access_log off;
    valid_referers none blocked www.sunsanmiao.cn;
    if ($invalid_referer){
       return 404;
    }
}
#SECURITY-END
include enable-php-71.conf;
#PHP-INFO-END

#REWRITE-START URL重写规则引用,修改后将导致面板设置的伪静态规则失效
include /www/server/panel/vhost/rewrite/www.sunsanmiao.cn.conf;
#REWRITE-END

#禁止访问的文件或目录
location ~ ^/(\.user.ini\.htaccess\.git\.svn\.projectLICENSEREADME.md)
{
    return 404;
}

#一键申请SSL证书验证目录相关设置
location ~ \.well-known{
    allow all;
}

location ~ .*\.(gifjpgjpegpngbmpswf)$
{
    expires      10d;
    error_log off;
    access_log off;
}

location ~ .*\.(jscss)?$
{
    expires      12h;
    error_log off;
    access_log off; 
}
location /nginxstatus
{
    stub_status            on;
    access_log             off;
}
access_log  /www/wwwlogs/www.sunsanmiao.cn.log;
error_log  /www/wwwlogs/www.sunsanmiao.cn.error.log;
```

}
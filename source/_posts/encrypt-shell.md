---
layout: post
title:  "给自己的网站加上https:一个快速获取/更新 Let’s encrypt 证书的 shell script"
categories:
    - linux
tags: [linux]
permalink: https-encrypt-shell
---
给自己的网站免费加上https，调用 acme_tiny.py 认证、获取、更新证书，不需要额外的依赖。

## 下载到本地
```
wget https://raw.githubusercontent.com/xdtianyu/scripts/master/lets-encrypt/letsencrypt.conf
wget https://raw.githubusercontent.com/xdtianyu/scripts/master/lets-encrypt/letsencrypt.sh
chmod +x letsencrypt.sh    
```

## 配置文件
修改letsencrypt.conf文件，只需要修改 DOMAIN_KEY DOMAIN_DIR DOMAINS 为你自己的信息
```
ACCOUNT_KEY="letsencrypt-account.key"
DOMAIN_KEY="example.com.key"
DOMAIN_DIR="/var/www/example.com"
DOMAINS="DNS:example.com,DNS:whatever.example.com"
#ECC=TRUE
#LIGHTTPD=TRUE
```
执行过程中会自动生成需要的 key 文件。其中 `ACCOUNT_KEY` 为账户密钥， `DOMAIN_KEY` 为域名私钥， `DOMAIN_DIR` 为域名指向的目录，`DOMAINS` 为要签的域名列表， 需要 `ECC` 证书时取消 `#ECC=TRUE` 的注释，需要为 `lighttpd` 生成 `pem` 文件时，取消 `#LIGHTTPD=TRUE` 的注释。

## 运行
```
./letsencrypt.sh letsencrypt.conf
```

## 注意
需要已经绑定域名到 `/var/www/example.com` 目录，即通过 `http://example.com` `http://whatever.example.com` 可以访问到 `/var/www/example.com` 目录，用于域名的验证

**将会生成如下几个文件**
lets-encrypt-x1-cross-signed.pem  #这个文件我的没有生成
example.chained.crt          # 即网上搜索教程里常见的 fullchain.pem
example.com.key              # 即网上搜索教程里常见的 privkey.pem
example.crt
example.csr

## 在 nginx 里添加 ssl 相关的配置
```
server {
    listen       443;
    server_name  xxx.youmeixun.com;
   #root   /usr/local/web/xxx-server;
    ssl on;
    ssl_certificate /etc/nginx/certs/youmeixun.crt;
    ssl_certificate_key /etc/nginx/certs/youmeixun.com.key;
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;

#     Load configuration files for the default server block.
#    include /etc/nginx/default.d/*.conf;

    location / {
        proxy_set_header    X-Real-IP           $remote_addr;
        proxy_set_header    X-Forwarded-For     $proxy_add_x_forwarded_for;
        proxy_set_header    Host                $http_host;
        proxy_set_header    X-NginX-Proxy       true;
        proxy_set_header    Connection          "";
        proxy_http_version  1.1;
        proxy_connect_timeout 1;
        proxy_send_timeout 30;
        proxy_read_timeout 60;
        proxy_pass          http://localhost:3001;
    }
}
```
其实主要是这几行：
```
ssl on;
ssl_certificate /etc/nginx/certs/youmeixun.crt;
ssl_certificate_key /etc/nginx/certs/youmeixun.com.key;
```

## cron 定时任务
```
0 0 1 * * /etc/nginx/certs/letsencrypt.sh /etc/nginx/certs/letsencrypt.conf >> /var/log/lets-encrypt.log 2>&1
```

定时任务也可以参考这里
https://my.oschina.net/tearlight/blog/738993

定时任务也可以这样写`getcert.sh`
```
#!bin/sh
/etc/nginx/certs/letsencrypt.sh /etc/nginx/certs/letsencrypt.conf
/usr/sbin/nginx -s reload
```

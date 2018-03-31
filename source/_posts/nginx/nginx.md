---
title: Nginx常用
p: nginx/nginx
date: 2018-03-31 09:02:22
tags:
---

## Nginx 配置

1. 配置Https

在`/etc/nginx/config.d/` 下创建https.conf, 内容如下：

```,conf

upstream qps_backend {
    ip_hash;
    server 127.0.0.1:9090;
    server 127.0.0.1:9091;
}

server {
    listen  443;
    ssl on;
 
    server_name qps.ribuncdn.cn;
    client_max_body_size 1g;
    ssl_certificate     /etc/nginx/ssl/your-site.com.crt;    # 证书公钥
    ssl_certificate_key /etc/nginx/ssl/your-site.com.key;    # 证书私钥
 
    location /  {
        proxy_set_header Host $http_host;
        proxy_pass   http://qps_backend;
 
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
 
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header REMOTE-HOST $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log /var/log/nginx/your-site_access.log;
        error_log /var/log/nginx/your-site_error.log;
    }
}
```

2. todo

### 配置细节
1. 配置反向代理时 proxy_pass 的斜线区别

配置1:
```, conf
location /junction {
    proxy_pass   http://backend-server;
}
```

配置2:
```, conf
location /junction {
    proxy_pass   http://backend-server/;
}
```

对于配置1, 访问 `http://nginx-server/junction/somepath`, 后端收到的请求路径是： `http://backend-server/junction/somepath`

对于配置2, 访问 `http://nginx-server/junction/somepath`, 后端收到的请求路径是： `http://backend-server/somepath`


2. 
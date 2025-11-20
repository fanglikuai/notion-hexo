---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PFZRBJJ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIAggfKg0o0sVLe173uLMsIXBQlp7QYeO%2BfJzuJB2MkkkAiEA%2FJs1dvlH1kX4I%2BPZxF13FiiTb58%2FfVhYgNtvd2ogy%2FkqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBoKmwRiLKX5sGfO4yrcA5k17z1sshgbz%2BuwjBbU9HfOn3jM7i6k27S28ZSBNZ4iSl2Q6qDNdZo8HG4wuUnI7KUbF9rSw4zzmD4AWVt6o9U99Ns9bHawBk4TJ4lKpphF%2FI6l%2FMhJrOvSnamr408evL4ttn5svQsjsb%2FNGUS7%2BC2GT3CnnupA%2Bwc0kFoTrLln%2BoegPqhHLfUFoTFKB7WIzSc2H%2FT2C7EhxD%2FL7V5Tso9qhszRkAkJkiUeuohwAmRAo9avaWhFJ66qMfMqSWWuM2Ec%2FrsGTahnGcvypVYbjaqkbXZ%2FWmhxbRMast3gR2X%2B6qxfXVbxnhVFgdciAyHRtczbZ9EiaPdcNbVx%2BXXk%2BtSzwhTqAXGcoFShldz9XcQ7m5ZxTh20XbWHdZICcMUp3q9x9vWEewfezLDfCINz8m%2BC0dtI7kTBMqyX3YYc6KOhwFZUVNPshmwL3Wy7s0xBlzP2duHgcYlzbx88vPvoNVoRyAPkaEFoDdNx78%2FoHUr0SsP4CT%2FA3Uv1Kws1%2BkbwLKAJBdVToZD0Yz2h22sUcvHMKfzlp7mAKixpxnwRWhK3oTowsHmfcSpGOx%2F5dEzskgleufkb38n%2B%2B1ZoDb28NfQpwukw1HRjxNahqxKth6khCLS4mnvWWp%2Bj6ppTMLTq%2FMgGOqUBBWXaQFuehHQ4pzqIcut18YYdpcCyv3WdarRzZXOrZ%2Faq16kjffcHysThlcgNaG6V%2FcyMyKYlLfD8XDsZPKkg%2BW9ti1X7Psx9O6JxM1LdtkzA2nnH6P2m5LzqvyXepw4T0sTdUaKv87owPjYlt5Ev0y9jWpyMw51Wij6rLh29XtQew1kyX%2Fvx%2F776hFRm%2F3s1fj8qaamgK7ZSSGFRNXOwUPhrRZlO&X-Amz-Signature=503c06d4a05cf0db5c6cbd82018cbf3d718b3b14579425f5986204c4feea9e81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
index_img: /images/681caddd167c86081c93eb4da2dc581a.png
banner_img: /images/681caddd167c86081c93eb4da2dc581a.png
---

# 基本概念


**Nginx (engine x)** 是一款轻量级的 Web 服务器 、反向代理服务器及电子邮件（IMAP/POP3）代理服务器。


**反向代理与正向代理的区别：**


正向代理：在用户这一端，vpn


反向代理：在服务器端，nginx

> 拓展：
>
> 堡垒机：统一的运维入门，带权限认证
>
>

基本使用：


```bash
nginx -s stop
#快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。nginx -s quit
#平稳关闭Nginx，保存相关信息，有安排的结束web服务。nginx -s reload
#因改变了Nginx相关配置，需要重新加载配置而重载。nginx -s reopen
#重新打开日志文件。nginx -c filename
#为 Nginx 指定一个配置文件，来代替缺省的。nginx -t
#不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。nginx -v
#显示 nginx 的版本。nginx -V
#显示 nginx 的版本，编译器版本和配置参数。
```


# 实战


反向代理域名的tomcat


```plain text
upstream zp_server1{
  server 127.0.0.1:8080;
  # 写要代理的地方
}
server {
  listen       80;
  server_name  www.helloworld.com; #从哪里来的域名

  #charset koi8-r;

  #access_log  logs/host.access.log  main;

  location / {
    #  root   html;
    # index  index.html index.htm;
    proxy_pass http://zp_server1;
    #进行代理
  }
```


## 跨域问题

1. 在 Nginx 的`server` 或`location`块中添加以下头部：

```plain text
location / {
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
  add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
  add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';

  # 处理预检请求 OPTIONS
  if ($request_method = 'OPTIONS') {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
    add_header 'Access-Control-Max-Age' 1728000;
    add_header 'Content-Type' 'text/plain; charset=utf-8';
    add_header 'Content-Length' 0;
    return 204;
  }

  # 其他请求正常处理
  ...
}
```

1. 指定的域名可以跨域

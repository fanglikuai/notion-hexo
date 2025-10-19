---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X5HQ2SCA%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJGMEQCIE5PWwUeyezTY1qEO8hvl5AwDt2lThh5NgbDWIQgc%2FiFAiAV9AQ8StsUVHZnwdusRn%2FqP6RYJqW4mfufPPOTFSBQayqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMq8MxMNs%2BcJYl13RRKtwDeSVsnYqo6wLNZjL0T8NQUaRe5tdNXG%2BQH91rh%2FkEAmV0pyXQ%2F4eaa9h1AZQgiSEqNUyK0AttOYBjVAu7cHHXAFmapwCYGi05eIfKbI9Ng4m7eY357q7UPJaJ3675zt%2FJspjYRNEgDwLTOuiZ6aigzhQoVAXytaV0ywtvDpjAxUmgC0ozOzzTetZCY5EAzs%2BEFGoMradZMxNsA%2F4XboUus2NUzmWtehmL1VhGC%2FYyJQDLsMGVPxmAiIbdEQr8%2F1TVYYRsym4zM914X5vrXyVAe82VcEhJ2yVnxOyUExrSTBLzQjw6MsIVS9KU8qMXRZqNsaSzHrZJi0XUe%2F5HNlkRPxEBeFSu8%2Bgb9ZILFmq%2FD4%2FRMRvIQGcylRKtWbxnTzf4OfD%2Btlwsx1E9OoAti%2BSulq9vtNyW37gx8LIR0jgDTZdxGdg5FgsbGqUZH9Whij5UYU6pFLWm2doFYOoY3aXHRjKzICx2NyBQkWACZYaSaslfibYh7n3gz7i85bVsTkDa9QlE1ljbMDrd1gtOOHEhX5TopMykXO7D1b%2FV1gOE%2B%2B0FSXQlknyZvH%2B%2FrY7ce4klNuVHZth6%2B9Ecr7zWlEou3KDcty34f6N9yPYekuY1tPEZ1DdOGbv22BmmOi8wheXQxwY6pgGibbSmXrCacGGPhTg9o%2BLotmFu%2ByS9%2BAWnmOHjwrAH39HzFy4uucqFFUOw6%2F%2BAOMVX9whrxg%2B8i62iDMH%2BLlMfzctQrIiMiX%2FEj3A0zcpNF%2BsTbI1zu5sV79bza7n7kVnqTJ33MP6RXbf5zGhGAy9v9S1yFQNzGLtUfQD6%2BoOpTA%2FcwsXKhmagvyr7Vx%2F%2BwbEVPU3E19mc4pqqUF5y1G2w%2FACHcRjT&X-Amz-Signature=93cad382f97cfe9121821bc90ff6151310b15d4a7dd5d2d770373bc696fd17b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

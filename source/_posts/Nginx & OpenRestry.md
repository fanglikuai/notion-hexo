---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663C3S2JXH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T090053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAsh9bW8udsh2jRVJ4wnGv0JX9ZGrvNkT7ybzHweY9%2FXAiA9KmLYhw40NdbJkC3eMqV%2Bgxn8hFYzXnA0P9JPdmHJEyqIBAj%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM38Rx0CMerhJNxOw6KtwDn%2FLpD61c3e8uASyDb9N%2BhFoQsjcPv02UkV%2Bss5kWmuaYcLI%2BCkNu3yZuWg%2FgQ8zIN%2Ff%2BosO4BbyHQhKRdMkKhQVHAurojVhMsjF9Rxy52%2Bgn%2Fkimac5Qx%2FJeGVnG3A9Gh0jpFzz64wxvgknUSHiu%2FwX2KuzkJZlD6WPHGHc9EnyNraBhMP3yKA2L9z3wI%2Btb2%2BvtJFSsIRhfD6%2FHEejkmyYvST%2BLK4zP%2BT9CcogcevOo7g7H7rKnnHXv%2FNN7z66euuw7MZZBCP8RJMHzU%2ByGKrADsdcpEjuTllCfdmLpje1f7cp2yZEn74mmuoxJFC2MaFd93igUYkscEP6xolf0%2BoBX4bGpLttG5HcsA96Lbmg3vx3ucnwCAz4%2BzrQJgyBtSJ8rdInzFLnUFaZvjKZancIkEaZv%2FcNstG0e6shFPNO1yUPa9N%2FxDXClBPa1alQf4wxuZx1CGH0CUV9FjQS%2BMRLUUYAgCWwibM0d%2BjkAh3%2B%2BO2qhOTw6fFk4i7PBkJcy0kQHQ9z4t1mz2IhVMZvTYWe1c8mm%2FrvpfrcCGAo3yMmvIJpg3yT%2FJYH1rYZwKXgJ1dBE4uvkC1Oppfv6l65rQbzlLxkVVK4g2oRGJGwq5phOApuB%2BvH7WWfdc%2F8w1%2F69xgY6pgGM6yGfuSrzolfAzPVbgWqW4rEkLarqlVfxi4%2BafGBaFLS1INIfEnfJofez5ytK9Sbem9nL4omUxc5Q5lT%2F20VO0JD61%2B6zMOr7ph2DNefdyJA9AsVKlJNCp1YSeyX0nyxSeLkb3ueLRvRl65QQVGp9%2FOBYrAr71CBv4txMQf%2BxNMxFlFRcfSbXhzXjnobOQpJCiR8go1Wnh3T7BaQ7pOe9PN1buqGc&X-Amz-Signature=e78c37594a5aa3bb4a68c62035766388126051a2c9d1fcb2baa6b81d9de8bc7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

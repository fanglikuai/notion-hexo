---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLHXQEZP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIBC4iDynz3tTV8HF3ySumZWDi48goJW8WVFTtZoz9tNvAiEA3FuFF4dUP%2F2yCSBJNmqdJpPDk9rVvBYmFh0b8Hlq2Ugq%2FwMIRBAAGgw2Mzc0MjMxODM4MDUiDGDa2TG4Dodi1qXiKyrcA1BXVwrBlZSSUnuk3JhOktqpNoxEKvzxwX4ZUGqId9InGQ74sG8aHJr362X6%2FurIWnXv4oSxU7IhyGS9EGRcDFgu0U%2BAVuSkB9Yr3z216OwQDBnvh%2F32DGb5l0zswDTLqGj4IP7KXIMtC5ikmc969gjPQzVKPAvUjeJ6hBl0ey5vyQV9HsgmZErVevQij0t%2F%2FTHOBn6XjLEVRJhxd31uTS4gTkZJuARqTnWQw1waJi4p5NWt7arwf1MBAjVH6VFvwPrFz954REZUowKkXaqzUcpbarptKGR%2Fl%2FVa5jLnnS0uDn4HoYp1IDXjPRRUyWIf9rY5RaVGvTQkntV77kLrlW6%2BaGKY7MKOOSbLq8wocIFRXBoCwJdGM3Vcju3Caq7OSvwi8%2FZHZvwPwsZgmCSl0pxQXa4S%2BmhM5QMsHgcY8LOrzZLDUZWhXGBkNHZAzPkc872GdYTBMxWQ4Onb7H%2F4x3hGANB15gPs7Y9rkl41YkNSFde9rEt2UZtVubP7bDluvL2OwMJDfK4aJLNqutcp0%2Fys1h%2FBI61ONk%2BfX3RUpeSzZXOaJLl7fhFUAQqvWIYaA%2Fx7mCn%2B%2Bn%2BXyGn0DqJA2bZj2kVl%2BvkAl%2FC35LdcpU4dNawD1vjEPl2NlsSqMJec1cgGOqUBZDkGzliIhxUWX8zYZ4bD7jNwglCriUpg%2BNKlEZUfURb9yb9zsnQp2q4OvGRlFzBz6VHwq%2BWiFGRGVNVvDxMddPl74sCY53OYB55ckqn7pPQnGdy4PTUqu4iWCV3%2FTsIpitph2%2BWykQkik%2FGHwXop8lBSQ%2BnBIO1%2B%2BXqzs1G5DGXnXFMNw40qNTMiPe7jLIhgAVs%2FwAUaWp%2FnC8gE4IeOjuRqdREC&X-Amz-Signature=328f72535cd94008a1505fa4ebb23d0beb6da554cc041fc1c06d0fa023fffea8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZKX3CXW6%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIQCG6ul5T12a14Th0wSLyT0Xk3up0YU%2FKTzHLtd9gRp%2BDQIgOvF1A66UgqY%2F1H7R0s9J4hs75Eb84khDDK8U%2FEZR7W4qiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2Fn4plqcrs1AgqwQCrcA44Ukzrzscb7UWtjRF13RmoYL%2Fw1KQL8%2B6Cb7ztHK4A1nvo%2B15CGs91XYgs%2F2ZKtxX6gw705q%2BzBfjEAe0wsB2lCB9q8r5DMCYRq2Z%2BFLtnlcEogX67w0Kh45mP4RKHPvVC2VjYMzVGMJcj9JWuedshnxHcNXuGgErcaCZVtD7%2F1%2Fs2Rcc3iktG%2Bli7Z65xfhugwMhFACSCwLBlvJ5klcXU6ZmAmcYw3CSWBZQzaRMlyUYKy%2BGsGbLB6hDS8W%2FHYjC4Eaq9FrhZv%2BWrEyaA6QPW27EvK7WrWiVKrkxrrZe4fuJy9bVNgkBUM%2BclkvA19ditYx5S0X8cwFq6hnh5WOhrdz9aVvYi6S3y0b%2BuvFooH2hFY4jW%2BpyYUnHDPWAZ%2FUBGS1JzjW3jz5R3Kvc6%2BeeAqjkHihi32i2VJnH106HOEAVPXPUdBjlacvErlu2YHv83LDmLlZabLwwtwIA5aQvUrsrrax4OdxJEDByePO1KKziZ2OxTSm0EpSPqfoULRQDudpfpiVUGvehYDYrtLz2tCzrtYCPynT%2B09FZxGcInHOnv%2FPn4J%2B8Oc3OTIlOw8jkdHObXNeRjkUWT3oArCITZW1I847%2BTzVW3akIVg0cwVosvdtyeHhdGhUO4UMN2a4sYGOqUB1eITAJrYW%2Frez8vMCwwlu5M76Pw30A%2BSucIbS5Yfv9d6zlOCSj8U%2BNqsIOFe00Yu5%2BwP5dXP%2B9%2BlfaHgzGRWOHSnFlVwHOQKjYhRkWki%2BytkThfyb40EmJe0v%2FYIMfC0O8w%2BW2YAAelWYDpgYUad8OWRvAA%2Fdf1afxlOdzgUF70l2cGY1JCC3OD3aTKFhGnYFjw2gGKIRW6RKwO1QoygAX9N451X&X-Amz-Signature=bb59557ee35b4bca9b134c8fc7b11ee86c5b5d42eca5546bee64c65b8d6236f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

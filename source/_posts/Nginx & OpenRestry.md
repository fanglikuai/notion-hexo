---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W75P42TO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGmMlaf9u3hMIsQVZrLc%2B%2BcmPypr9Wl70O2eSeCDcqHqAiEA%2FFJcTMSgR9XRNtcFHZBgWCIR1t2Nw08DY5dJEXlDduEqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHGbYm4eMf5JhDrsgircAwmgdsKUfz2sByb3KMyfPO6GCHH6alXc%2Bk0b8FktR%2FMCJbHOwMDjg8uad%2FmA3qyoXknJj29Wo2HmJFkokpowwK5pMBdOyBQAlHvpZv6KyYB9PHwDc0H92m3v8aneysVxqAcIkcr6YSG%2FM0VWBiJ5jMfl2uVFCVgM1y%2BZQol4CjxTJ0KOlCx8XB7fn0uBskPG2vts8alxHcn9pwcflj%2FH0h2R6Qkwb3IQ%2BIILo%2BeaUNiwxM787oZZo5RQEt7fkp83NUo7SIrGcWn5N9JGr3%2BWK9EK6Ln%2BGPKtxptgb57vlmBriUfa6D6s96DRhCcNFeHYN3mGFD7qSL9L2WLr6hciJDgsydcT8r7PDtxyeEN4q5Yoj06Hj2ug3CEADx176KdaOl5tv5y1jWfNG3McbieSNgi8L4YglANhPItC3MN9t4r3ITY23iYXMVUOcctzJelxLImHT%2Bc32%2F5AkL4x6WiwfZlVriLiUEm6q5tj5EWA7RutOjZ0YQ4gwkA4fl98hy60E05fZBXd17vWRCSEB%2FbXBCaVBW886gOWUmKuONOcYKr5BrHvJpZ09nmi7tnx%2FQGwSb%2FYYQ%2Br4oXa9j8B0rgF%2FAZxd7KafLA0mqbjic17dT2BOtZGNkeD8J4i%2FYzWMI%2FqnMkGOqUBc6B4viJEF1yLURtxJ9ASiASwZHCAEYTCVF%2BJkisrtXzLsH5t1Vsmq46mqKhTXzyJpxrletV2R5aWEp3Q%2BQQ1GUWlFFnjHvRV7SmcBO62LLvXKN3KVI0mRlYsiSxuO7BuOrUlzvkZ3aupF8Rlz1ur7rk7tkPnTedLc9BRN5h3X2BlFv7CxbR1jqQWrlrAT3QtWSnswcDDTDAUvTj3IO5k6n9yDqhC&X-Amz-Signature=4e3deca07c8c7eb60020b3bf46b9f56f5f441b2045a5e608c9dda6afa4f2a9fb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUIFQYVB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBYKV3zvaEzQT1lHKcaHaFXafDm5vKfiAWYcBB1iJ1SeAiEAg7oiMqw0GHsYY4QVC7EPjq8UuMfIz3hCR%2Fb65pUEazEqiAQIh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvZD5oOu%2Bs1OsEU9ircAygD3%2BnEVORlh4wP4RE17evRzhCpufXJPsPDUDnscnKb1dPK97kPlR8ReM%2B0AXuVkmKo0Mgq3M%2FzihimxKNWZYJLbmQFpZo47H8tgfp8VtHGsjP8pEIZJxcaUYA9FbxRplC%2Frr9kZ%2FFAPEyBULsYsz8045f%2F3OLMXWx%2F%2BGiY2UXIqfEbmXJ3EH3zgiN8QqUY%2B1BLH%2By0CEHdKJslu0y3pschGmpO8%2BG2woHvDt2KJLFVvs8EmGrKNVUQRwMRMMtheLSZBhvB%2FS1oy1J5kk%2BGOpES6ZGIMXYp1zcNQYZBDTQEilJAEHzKMCCSfrZrqPS0qBwLZcxf2bnbD8cZOqyG9h1qC%2BeFc6Ox%2Fkf5%2BijkIRcIHOg%2BBjwiojzLfpSbN%2FUQ8CQLhaeVbg640stiENjH%2BDGXmIvo%2FrxV%2FzH6I9y7URt14G8rpUM4Yz7G3sKe5aynugGWSSQyzCip1y2OOqlccqQXoKPR0bKA4ZDoqW0ZUV5qfw2pv30Uhr5zRYm15cmzky7c0q0pvTqlmHWEXdwf4JfERHhA19n9hDnUm1COg3kWQIhXXouSopeaku6ttSNsVVCfvo3zj2d4zIcgcPJHx2YVzLfYJmTCreB7yRrAxnVNuFLOrKNMal9%2BTswsMLCNwscGOqUBm98vdtqBwS%2F3Ntsj8Hnkcv2EdHLAYiAyl%2FVeqW1GUFZp4gmeLwsR%2FaVrNd7SBlhYNiPmzhBPAasy7FLRmW578j2NC4bjcOCLOFGvVOMe1eXPL5S5py2s9n9Ccgj1v5XVCw5v3ZWIKI37dZ8TlfbEkt%2Ff%2B5nGix86BB79hW2xMtmAtTn%2FNu2zwuEVFxPaLqDii5MaexKkpx%2BGQ8iDYbdQ6nrO4W1Z&X-Amz-Signature=c1cc7974a22c9036d09c38c0a035136f169e3ec164e6aa45bdf4e6b96dded000&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QR3XN6A%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T110049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDnVRpxS7FM2jSITo2nJ02JIfF6zwEL%2FLuXuz%2F%2FU5cnGwIgDPNgpYmG0Ne9%2FauAvV3vr6jjFsCMiTCASuEtBvq1zh4q%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDAvTgw6lbyk7NDGJZircA4hwhd%2FLoC8bNR%2BVRDecskI2pCAIjpUHT03rIgkM6Nk3lb66g1NWYfX0pU2TV%2Bqi5VmOiP5hXkKE968GpKGDQ5l2ZSJmct798%2BsaJgCYGYCXBZh0NzkQqDMGB3wMRPpqL9kOlHkD0lPXXLYYFKc%2Bwa8Cl0QPF9fz5nuOf%2FzIiCPnUlj9FftueaZVCqWn04QuV%2B01s%2BB1xqTLuVKOpF2xq32Bg6970SpzU0qU3rbiy8nF7rHcudThq%2FEAh2%2FfoYTLxbvmtfqDou2c8RJhRP3dCWR5nwMxWUMYLVte3UTBRxC2njWMwGblhqmH9U5eixF%2BT0POM8pubfnU9uFDeDjcNuVAYYTcx67ToA3xijAn9nVnYLFe25vra8V3wzu9maezcCWsam8tLHm%2B6%2FeMUC%2FBahi%2FIDywwqQrMMeHm8GPaywxzrY5IbmtlH2e0S9bQPvn5CglMzNeNBDheS3od5FsvwGrwak18gc9pMcH%2BuYTS2jmxHLsIcHh68MMKQAxdHmd68aA%2FgdmPzwj8Zsz0jo%2B3%2BN91YDu%2BAOSKqpRDW2DGYfXU0kZylsnil%2BpM1sy1UD5Pjo%2BrS6UY8RyqPbdxqI%2BKf7RsO%2BhLKoWSsA4n52CbFgiB%2Bou7B%2BFmliomk%2FDML3Um8gGOqUBeTOfM6xCGfY5DhW%2BEQgLlgvFyX82e1qVT9hCZ5x3oMYbr8k3R2N8Y%2BOW9xnBfkFcnA4nbrb7fwKV0g7cGGaYugGNlFQiJjPAvzJNAxCPi1%2BZgfUsR%2BE%2Fvnac4TLRP0RLb6VQa8WDu9WsFWKigScvwHutY7pQHfpp9XIxM1vC2Tp758xLTr2DScs9RwD1qe%2B%2B2UeP0FgLZG1INs06XqGlMeUVqnMS&X-Amz-Signature=dc4917c74bc0b56635cf96da9e5304a97987a936c4fa5c4add217af33566695c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

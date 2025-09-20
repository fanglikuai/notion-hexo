---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46626O2BX5A%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJHMEUCIQCM5Oaiku3FZbfGRD4Tvz%2FUxEpppEE6gAIj47snS1OTKgIgSheOCYIaSVV%2B0vUnN6mat5zFH%2BqDEJxu4TGdaJ%2FKMLIqiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBxVuGPxJ5OJTO55vircA242ASJd3crMINoRtYQwAhsXTUshnFIye7fH0a8vBM29vcXYkThvCFw%2BVqk8BB3giWSpYO%2Fk%2FkTkGEGaf2MJc2GzAgNPPpFkGP%2B%2FOLzKoMzSwFMCi4FwE27X2BWaXPd%2BbzABOb8%2FeGKNReKtYj2Lm6q9GTUopcDTgFWMSGTtsfyFaTkNzlIPiE3hCeuHlT9PyWqkzzR4DSOhyXJhOqJrPa2Dqrxes426Q6vaqe8C5er2wklWsVI64ciRgPvYVBssnALKV%2FOK%2B7aALPsgbB06Wx3%2Fa1UIua%2BCvn8W3XdpJ4BGhFkSqczK0tcaSnOXwYkwK8d%2BIqzdbqsI8iKUep0MiBKwKCKkC9SdHEGvA9qFZCOxH3loB3%2Bp%2B1nl1Cfx%2B2A%2FjO3NP2mr3A58jm0gBuzmf0o10UDpPnuARHDjqov%2BMQEZXNG7T5lGA4nDaVbI%2Fk2VvZ3ihC4f9WiN%2Bi0Q68W8%2BFsKA0hOHzXVLeK1VwVrz2AFqfkCXEx6C35423sdqpJW4DVX9a%2F%2FVdMzbeKVMVKpwA9vyrmtQve0M7nIm3zUdpYqbBVBkujpwx%2BHZaMQ58pYkM5JRRu9yA6G1ujcJveYUVQsoTm3qX%2F4WUiP%2BBw%2BPXjF%2FjiHeXM4uITIL9VVMPyLuMYGOqUBvfkznWD4RDo5GIrYw0JBAQTxXLI1XKPNZ7vCFueRo7dOND8yjTgL66ZyEUZr7q904Ndo3NmcJoBn7et75PHcjm7EY%2BFqJObR0nzj1gxMQULZdhd%2FgM6aPKG2BeTEXIYL9Szb7cwVMMICsu9bc6ZT7qShfh8GuSX5jczZdu68xJzywReUVBa7wd8XTTMDRAChxMaCVReSitAnD8gosGKEz6E%2FFjFA&X-Amz-Signature=886ef0891d148f3f1647f8e06b7e1bc5cd49bb388c1a258f469a5525767907ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAC2JUE6%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICYosZC9DhZlz7bnqjT%2BH%2FXLG4b3QrfYcOjUIfR8jnksAiEA5OJ%2BDtPvRvTsxiRwfP76xE6%2FsN0vzno5wp2hWYBcM7Uq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDOyOapd%2BvZCIv1gS4ircA4g7mpfIioWHzhu4YEJbtvTx2igcHzW%2FksmwJaZ2bo2NgYLr6wo7z7spBWbn72CSWGXh1Rf9Ur%2BNz6AXKFuQFNL1960y8p2Oe8jhXgJpGAG1njnw9LKOeOaeziNFJZHJd3YTZVS8eMWBBMMK0MPh9zylz6%2FcDtjJcBzyEGSE9Dbdcam0fX%2Bth3DgQdVjozkB8SqldtX4gR%2FX4POOe2H3mMEJFXIEneGB6XCgrXY9Gvp4MKfwtLbiyZOk6xI434ZaydCDS6NLChIj1hm86Rx129Nao7z5n83FTYNfYZiSWF3EEyG9fc9XyuLs1QS1x7cfWoYIaiLuWlJgAPHncIfgRu5dwWc1izBdoua27VEcl84zx6P%2FTUsprOBeMJdjWNf4r5zyR2seXzrDRLTCDjpceyVvPtMNinmFt0%2Fzbfp21oJyUsNq6rvBqV6pUzQG3Zhjb5fNd0kax9lPU33skQwuml%2FCncWD%2F2DIZcqwOCJDadmK0KlSOZGjtoSxsoeAVMzl3LOvfGewOcy1ih%2BjFP5yeBOwg8TkZ%2FXAAePYiSVPjc144bozF99o63jG9s7pZ%2F5eT7OigKf43FxGzW6BYD3nbeIn9ySmLqI1H%2FJy02wX8tcJx2buAb9EmU1BVX3CMIGIzsYGOqUBea%2BvgRtcStasRqekygQp6q1kSxaatM92jjyGpvkQrFjDS9h68YtShM8mqnd%2FKp31Yp%2Fbs86GNjE55VGe2YWAvPzp0%2F25vtawpRyJFb%2BcYiCRQu6%2F3KheOKeoP0eO6ayGqjtFKdVXgarthCnkuzW7ogJmZ5J9QvwW%2BTNDHSVQ3e8Vx1XxegKSF%2F3NMxM9WJ8LVGBpSOnD%2BnG3JiH5yMpLY6SzTzE6&X-Amz-Signature=5ee8b67b2a676e6e347951650750943683ba2ed2142397c0a1b2426c47b55745&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

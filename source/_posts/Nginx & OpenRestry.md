---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWBJQPWG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIGioOW%2B7SYIOFsbDp33M3hyvgqkffsNUnMrcAhsOiCAHAiEA2Bwh0PnQQIWQCpkcH0%2FQiCNk1fZtNlhc5OBAh0o9cdoqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDXlK8flbNfg9YwM3yrcA7ETZB1q2mTeNZ2vpavlx69Lbt4D2wI%2BApHgU3B%2Ffv8684Kd2H5Birw2PCuQ5TlYkOAWAlnt40FZBHVEVOz%2BCs8AB5jnDVaOaj8dBewoNuKQjB1tWHCpAqd2wx5QL3FjQGrXsC8H35leabYqpVzd3qtEUpA%2FggbXkv%2Bd1bBM77f%2F0HFzfyO0uxgAOnC1vfZzVAXXCPex4l0h1laWWHOnVNjQCkWM7jo40DsVIZfFGE3eImvvP2X7oWPTEw%2FAPQWLoMob0iSWYaoVF5PPvIpKhWvE3iZ4gEK2wIzCg2H8GOAhVpRsKtk12xD7r7vD1S6SQ6N0ZsNQSh1SIvG7yh8Yi8suu2G61KPOeXrx9bnV97WjfBh%2F3gFLnNrnc%2F3egjQ9iwBm22x8PpZloQoczCsDjaPxsiEEYkbGTjsmbgvMRrLzEFyzybenyqEoNa0daQztXsXEjU%2B1VNPPmQCWDXl%2BqQSYHC7nrql00o3oSxTw6Gu4HGlqxKkC7OqiRdR7%2F6%2BFfYLXSLw7JocSxJ3PCXVydZu%2B0a1oWE4W8wFVUc3Rwo6UWhe1c%2BBj9ujvapJK9KrHYrf7bH9yF0dVnII5FjodrvMe%2BwmeP4%2B%2F7RM4MhV9LupIbnaZYOWdddzf9ZS%2FMKPip8cGOqUBxcVuKsp3LVJPXuVUIceYLGGfhK0GO6q9XhnTW%2BwseqvJOliMOh82s1T2ba%2FW5RaEyBFiX95z7ssdF%2F6Mm4bT9TYueMv2P7h%2BIMgIaLIZAvv8X9vM7kzXbRq9wMTdtfBzwAB0mumR%2B8H63DFsWpbO6SNN3VDWcc3uUyUcrujWI%2B5aSxRaOoTYxxHeajkIkjTrjLOwPWQ2k6YKjqXDJXnPfb4tsxMg&X-Amz-Signature=3cd9e8614e488f06c2a08dfa324a23e6f900a32a3d9752b7bb21712c3d4a96dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

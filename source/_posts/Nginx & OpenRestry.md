---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QGYHCXCO%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIQDT5FPE22QYV%2B4fDZ%2FNNyh8xVp1GcwkDrjFwKXFmiIq4AIgFtswhwL%2Bd3vtuL%2Bd%2Fm5VuhLgifZI7SoULVaZV8qSsdQqiAQI8f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDNdsmnvrQMoSQzMGircA5JaXTUOPSl%2FRfjwPZFk5IFEhFZe6zm2e9%2Ba1e8MqPAw%2FQMH17F8tmSJAlukjhSgm4lLLw6cDm070zGIHifMHVBBcP%2BZUasTnq8chdoxuM0pdDYmNh43VKsq%2B%2FntFplNp3RcaSLJXLZNtOANchqanXIZAfrsPASsdDidY%2Fifv5crx20qkIAW6wUr0K%2FUZezcnzlYNbySMISz1X2QaoZa23%2Ba6PNAlpbDHZ8V%2F9iPv8A8drD5C2wU6YzBGf1K0lMsomCaAMcu0nO4bebuUl2%2F4%2FlY4Stkj8Ik1Aw0rSgYedI9Dhlh%2F4Zo2pFJ%2B2NfsadSbdAI1L1WnqE1YH%2BkzLlpo6KGwSdKY4DMuUV6zutcWcM7DRDXP91bROO7JWYmbz2OL5izvdfQI0riXrJ5%2B4okR9ZKCFTpoQPISw8ANrem4j5JB1wAvnipQwT3fbi6wzhVK8e17XWNzVUfJBdup%2Bgu4M%2Bho3uuJ4UsuacSuqShyzBgOz3WUEx%2FaYDH6PGExSfWb6iGCDsQGQVR2LF6JZ4R%2FI1x1meBoGb5b%2B6JmHAwyxqFJJZPvQlg9VKeX7m4sI3AGHel7bwLXrxuobZxxXLcSaDyK7H0CoIlCr49w7dA4%2F9h83u%2Ffu8pRcklCHcYMNeAw8gGOqUB4wH9L5bHymILTWiAiWBnbPJxBAVFjldzW%2FsGbLkeL1BvQWce%2Frra4PcbHuBIzGZ2d2OBjwdG4h%2F2YmEwxs%2F%2F6n7CXd7X5AtSsX75bFrDfKbM6rlJvXed5BaeM2bWC7dz4B2JqJpn%2B7sRD8oUTVIsWUgnW4NpvZQ8nCRzuZEMaY6odUbfZuB2d5nXrVwGLIvOztODo%2B7sJud%2BTFoOUlC3A3c0Kp3X&X-Amz-Signature=a3dcc307fe3a1334af00ff254a58823fada3d2c1654f2db2c142c3e9082cd9db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

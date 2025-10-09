---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZM3CLCK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T140118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJIMEYCIQDGipmxiw7Osvs%2F2GUzmQcYllV8RhKe6wlw2PaH2CFtGgIhAJIZVDMqyEQANFjeWv1yf4w32S9ssFZEt%2FsfNEK1lrKqKogECNb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8em0mz65Bjnr3QDcq3APYJrepqERYaVUAuFwKtkXfMHGbrJMxT6e5ADaQbdjE%2F6bijmWgnbQ1hVrVdKcL4lxr1AM4u3dlXJ3JJgCoU814pnUPX0D%2BlE8LVmvK5jaTnPEs6msdWV4PNlxXj%2FtxxmDOjcjXj8pmxQX7e2jp5CXHqTCa5PASNnJRzA9Ap9VDekYa3F8x4gdrHrUO5alhebZ%2BpD%2BiNzkahbKtk6u2Jnlc9IyD%2F%2BQIm%2BOxTMhM2lLQ5%2BTB397PVgAV4eLg3wKzzJjlJJwlIiajRpNR2Klk%2FypegVBB5n8uc1JuwjdgZZYjUqPBvVszpt6oE7Lr3oT81lUDP0dFo27Lv%2B5rCSL34ol9ouuNF7ISsbBuVOS3wmCTg80syfLQAgtHhMiywtKJhPyBckcRWKHqoAj%2BbKvEPBVw4I5dAq9qvsSGAH%2BC%2F0woG5HeCphi0pL%2F5T2xokFBzNHBTrVLnaNaWCxkTbDGTtaIffLedenrAZ3tBv0%2BfC1tmYHXHGfMeHBRJv4qwPwx%2FfrSvp1PXQNa9eyvr0tlblv5BZicv6Q7LNKixBdMRhY2hHFzUlfcUW1mTT4u%2FvD48lr48eKxpntaT8VOaXqT92x1lcuZkWjd1aVwXDCqJjk2rS9x5vJyxFBy8zDP%2BzDL6Z7HBjqkAT1mf8taAoSI5kZ%2B7CIgE9YMqUBmJ5oO8ZmNTgd5IkyC4lzvCCNg%2Ffmm%2F76hDaLMLrp3VOdlcR5PvtCF8aWO9XpElALxrSuUVrcOnkT4L%2FCHnjbLmd%2B9M2O6DNPQ%2BgNpcgEseFnM6MqRv68W8jojIBOBOcAx85R0py99yBV8%2Fd0rY3K73Q1ezg%2FcxuLKKCpY0ObZNoL%2Fy4Jo7NY8x9fxSDZjK2QR&X-Amz-Signature=6f2be03d8417891b603c63aab88b406dc675745dfe2080576663acb275e5877a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

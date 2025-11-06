---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PLQZFZ6%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkpprREjZgcT%2BIMWFKaHTw2HZ70cBRMmpXO7ykwQ%2BJfgIhAO5NVTcIxkJqwimjfF695Vlgsr0Ucg6u0LuI%2Fad%2FxBa3KogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxAWB0Gc%2Bs61VUCR%2BYq3ANIGLqp%2Bf0bMKwRzUlxZHIa9tPx4jpDVCb%2FXeoS7W0r39HBKX2VxtV9Ahffd2n31MBzMkcOd6zU1%2FKAn4KSBApqUQ%2FXJhzDBBqxJoSXI925sQ0GFRQ1wqOJVrHeVNSEdCCa9qeJNg5oDsNfHOH%2BZqt8mASXlLkjitJSPlfPjIpL8MyD7494xMQx1d7xEfM3bj0sBudVI%2B6%2BEfiafhp79N%2ByChMlC6Gpn1tDggyN8Dd1h3KelWkhR6x36dCaRuAINkq8OBRoYfDZLhWvrDCqn%2B9PrRjBISufLU%2BagmeNsBNjsvTqzDzlJdjSkGOvrtZ3Pj9vYlIiau1Gxw5ZjwdTbAsyU%2FEs%2FxRwHYcUkwVWh1y359BL53Sfr2jrxiYV0SabFELIcBaE1g00zIxk7VsHEdoPUs3X%2B0cNAyWvPo9M1UK46telXz0qu1KJZc1OUzfP0FGOCYdNNdbXiSn8nmPgOM6OZrro%2B%2FThZzH%2BFPLDSVPSpKFZvwi3H8d3ywkR4eFzdqjBdOWn3g%2FNwGTPNcHXTxVv5klX4WiSuLD%2FOIyKEUoAKlkh8Ozo5L31MFr4dFQv30qHIT%2BbcsdyftbWfrKSA8gq0mtP%2FEBx3UXMxlGBAkbrjNSLaGpra0ofPCjgGjCpw7PIBjqkASD7F1c54Xze3ja9xoSsRduTkEHYklrM36oi%2Bw277sSWo%2Far2qmnOi9myxoHOs6Wubo6gncQbr00XoEe9u9qSqeuJ4li5lhw%2FnjYvNvVNiiu3OF%2Bcs%2F3bMm3CHkUI6GE7nEUpqEeAh9P1ybnVUY6kfojXPe9Tc2sR549rIRl5v7dMaLgaEZr%2FZZkgtJbJFI%2FXHRFYVCU5lA%2BxlvgR7J2uaQkaTXX&X-Amz-Signature=af694e8d19e4fc0ab1ccbb6dad3e4fa764016757ca740ed0d83ad34b51369ffc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

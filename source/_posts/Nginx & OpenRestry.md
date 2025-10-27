---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632V5CTJC%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDc63EWCU005y9Dtw6iQNozxUc6B5pi0wCIDRoJYdoHwQIhANP%2F%2BuiSH2OA9f%2BvujEvP5dp%2FbcAgRIiZVnKN%2Fh4qQpYKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwByJGWdc8Kk8be7Ucq3AO3GIR2qWX3BXHRaeAo17Xrp2UPY9riN%2FC4y7g3TTANOKjgRIIjyIHLziPNFd%2FuGxSj2eH0IJglMTiIhsMr7%2FmUbL5OqHBiZ4CO8i2OYPDU0p2HYMy7%2FIWTPeEONREenChZ4lj8e3VFOY3MnmB8XyEGk%2Bdc8bZoG7D9DVtEVr0nrFpEyT3J0FbvpcA9EtFFbq7woeH8wY1ERUB%2BZvJzIJ%2BOqDBf%2FjiWettw5dO53B3XFgCBBbX92BmJntVDOXPD53zEROelVzIeIZ7ygN3m7R%2FcjrsnYxkpsMOT0KlEKNpQ2kEToewY%2BZmwcgDq9MVJgus0UpCBv9z2TxgLU5pWElllJihOmQ%2Bv5BFsR937cV0wnnHUG2VPmW9ElfimiW1EzEhM9u0zA5uJvrUvmMWn4RG3tur%2F6jjOMom59fDo%2FldaOLY3TzNnuLlfgj0%2FBZg%2F1ky8h6xfJ1DRzCYHlSuU6TC27UNG3sHlTsaDAt113Zy%2FHRRLeRpBmyZeJXE%2FeZJD%2BGlVAsfhwGJ7fYem%2BE9%2Fvx1rQrNYrx2XRNVTMg2zEZwFae75j22%2FTQq0Pp0OTLYjDNpm2oikGmbX72bMTASsa1hljMkebJoevqSo6uTFUJowYDJfV5N4rEXET3sisjDOzf7HBjqkAZXhYQ33H5%2BHKmrl%2Filnmh4O%2BFK7t1GXxODNs3QVJWaKfOTK6hZDrbw9PMDIFIjOPnSaUSGRq1b81SBwQL34qGfXReSC3CueDaBxJVR0bhZa54qlSrTULC9bI%2FUs7bvg8roMeQGP7V6zfXdfp19KqjOb4Wz%2BWK9XXruCsfwOF99f0la3yjHbm8DRqRKW8%2Fqw8IJY%2BmvBDL4%2F2H%2FWG0jgFQRzuARW&X-Amz-Signature=6c14206d0c63a545400b6046d83ed6168b0ea26019d4a4728cc258cdd2053a04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

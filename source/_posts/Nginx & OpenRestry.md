---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SDTQKI3%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQCHdryXGkbwOIhBo6NJxCeJaO71nNPdePMFlkF1eOKIewIgLpgnQmP78tjq5RXqwdJviluXUyQJhh2ToKJYOCbS57cqiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCygbbrESgfW4DxOFircAxSAsP6TVCk7LsXNdRqk16l%2BnZy%2BpgRd7ICP099Ey6qnv6YT%2BpwvGJLANuC0bXm86BiraH3eAb%2Fk3lSFF4AraXBlFwCupBBxL8PvKlrWV%2BKT6tPXaCoAJc5SOFOdmOw%2Bk7H5aR8qZDOzayLNluDwg0GcEs34NfcIxxePlzZwzkgcEfY67IeWtTrgKOWPaNduBP7FV%2FaVYZFGD9E05jFYwrtik8FwTMivUP%2FlGji8Rw5ptQ4xN6GJRqthBItrI5OB7BQMqNCBojpbQUBguJY%2FNlrcKcdEn66YqZgmDgJDKMUAxzav%2FHLB02DpEE1zWsk2ZgsQivOTLuzriKol%2B08eWSTsKKAKf2f3pSLSBI27dpnJcLX3C3EkwO5H%2Bu6MpPwwtZ8wafI46aiPkBfeA5mss5CoeObwIVXwMK6QygSSi2kx5dfAMghHViTpmOfGT1PhHivPwErk%2Bkh3PpuUPm9HcWFliQpYuYMFdNzaojp2JuI22p%2BKFAbgDHrBuQlVnXydTuearl%2BObXnXU4YnJD2lYDDa8Z36MJzEaS3jtcNJJl0dH8ndwDB029kxCHJ1qskfwQcHT62kZU7zmvS%2BhKMrC7DG%2FjTkMY38IuB5mddWzJ%2FC%2FDP%2Bw4xGu9%2FaOeXYMJuy78YGOqUBRe1xmFTEIYFp%2BkScGRAIf86rHk6SW9M3GLaeuMedwCjNoqdCuARBKBvFAB66IjITJxE65j922gD%2FdYsgf%2BHG2NlOOiIbP7UwccVmiz0FqzTx4cWz4s7tnTYTUi3BOQGg6xcRXpndk32jA16OUGxJnnI99W7Vj%2BeqxcstngzjfPyAE1ov2XVpeQF4kM%2BME54672QpgKWQnLZGdnud69ZuO3A3B5Ff&X-Amz-Signature=f33a0cb08bf4679370a768ff5f6af124dea415037ad9e4603b0b130035fb7472&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

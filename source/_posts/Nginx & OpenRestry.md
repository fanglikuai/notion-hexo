---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VH4BCBCO%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T150100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCICJrBiz%2Blfj4oID3ymn4glZ7bIYh5o9zaHxeKvS1piyDAiEA9b4hW5TvXTJzuQZ4K%2BGRkn6Bl5uOkrPdt4jW59b7n8YqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDK%2BKNZQ%2BpOIVx4TlEyrcA24FGF3OpDbAo3Bok8g56yQb1FD0iQAvKXtvXhLw7eA223RG%2FJwXFoyEsqcn9vaWPd63vYcdGFKgIPkkRjkkdobtHV4fa5aNflCQFz9woVarKAdMztatTvhe3Kg8YI%2BIca8u1k86kRDYI4%2FS7KIhILSAyKTKTJv65HSwH%2FoF1WUU%2FFuE2HwzCaCUjGF%2FukziBNuz4w4GF9eVcEPcKVsm8GYJUFv4Yg57IM5ZfN2VLuKKuq3m33NAAK%2Btn8XTBzoZ7YGieMDes2kmQLubzGIQq8umuJBeHWBLweJgs8MU9QwiPWL%2B2tCWYfAl3PGsxFv5sFQneagLH8B79FBa1xRbC%2FqQtoQdJsXCAsNgZCKOXLUbDDHMr5UajybKqLOVj06led4Z7YCveK8yeVbZRG9PiSSNIACFXfMtkNlf7d5EGVOoiX%2BwF0XANQ1YPFmvQUNCQ%2ByRZo8VZ4nXCk6JcjPQd97MWCk%2B0eK%2B5bMhMf2%2BYY0OYxbpC4WxZP%2FYhvYG9e%2FGFLf5izMQOZckpp7QHN95PtcINr1f4bAJT9i3ySv9xM6kzxM0pCvHLUIg7OCpFfq24yiW9dTF%2B2dYEtiwLntIRnCzG1gDG45mCneuOc82fmY%2BbCMepGQ7H30et7iHMO%2B008cGOqUB5Af%2BmYPIjxY0npj9ztoV0%2BxciIzysqXKMTO818kZycLqvnQK1nLT2sA20a3TaoYXHVtZ92rsf4jdJUETEZCYdmypwurmfwBJ4PUfBrYr2CK9%2FV6hXcEF%2BciOjLTRYUrys33rEJxUaR2UF211TDoXcSzneveA4%2BDeF5UCGEqRQzWBd6Kg7jQsJ7iVJpi17%2B1OFTiWq37qjDAyaLuMCEKrE7aoGTBB&X-Amz-Signature=9c06da63d089dd3963da0eec995b5c49a467012dfda85774b7362470b32b0083&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

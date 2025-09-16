---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3FCQKAN%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCICP7RqtV7qj957v0qZ0GxreuEP06NLJoKHjFbh3q39NKAiEAlRnTJoVUM%2BNUzZ5XdEfQ3ozLeZYceoLBAVBPtOgJCDsqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF9bPm63QkTpKoQWJSrcA1n3UoyC2UMGdK6ABFqieVolAWHNDWsqDMEGns8%2FKdzmms1b9JkjAWbP2G1emPg8MoSUuV7K1e0qYTTObvoB8YOb3WvQ5W89FjUZJU%2FD3hJ0meondfOiQT4wuIKohDZsU8JO2BRaiK7HK48pG8%2FSSCF%2F7aHaWVWJS07YPfiRdru4Q1jUakMXv6bcg9W6KO7Bpc953V8EikBvcdDh4mJ%2FO8PVYKO1p%2FnJhIvh7r8LWsDMlSlCT4XyEg9DJeD6C5OncoMkXe8SVj97ZTRQjc5VEar4E1A1VTncjRCK6m12VF0YlmuQUiDRc0YOoqp2IrzAijGJdVchic4ppCQcHIXpXS%2FaxxxC7YF2cuJFinkekUyaNQbVXRw2J%2Fs4sAV9P5kuhHykLxeY%2F9GsxXPB3Cdoc%2BszK4uQr8UKjItZ2%2FtgaYD1%2B0O8A%2FeCKovL2lUCkZLI40wcGeSe%2Bgv84pgh%2F8MlXYF9%2BGeE3sMUDb81Usuc3Q3AGkeBOoyC2TCD0VHqA7QAUPVbEYZ8CdqpCqjfhL0%2BkawFB%2BIKyWmJ0j5xat41hUbaw4cY94VaAFdTFd9LopQLRFKw%2BCMOwZ%2FRx3hlyh9PiH28JyXlYB6POrc3WJ%2BstFH%2BpcE6c5ivGZVPBCmmMPmYpsYGOqUBliJ7CMAwCSLaKilqp248mWIfPAXBj9pISkYlphCrWuoUJKsEg01WM9ef9QPDuOWt5Mmaz63WLAVBJyupcK8CiW97Zs9lvJ4wI1O2BF%2BxfELlaYPoC6wGGSBzbQSVGmuG6aWPR2yTP2cq2P0SD%2FawiAewyCdmnXPpIEbXBLL7cnOsqczcONrQCXwLG2XGsFnnd7oAI06tL5yXZxbDLffmIDoY5%2FdT&X-Amz-Signature=2a846a85f19e401249905c6d51cd66394e1b6fd315acfe27f6d0040921c18399&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

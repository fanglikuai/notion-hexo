---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VV2PGU5L%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQCAz%2B9bMq2XHkeF8a8caR58c9hpPbK2zP0nBZEk2zmrkwIhAIZiblhevLO8%2FdGqZX3QntpwUsvJXk1DVCTv3NKKsHlyKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwJxeRhLCmVdJ9g984q3AO8cBYogSC436p2Tx4c24IYJOOYtg4x6GNFjlLoI4C8TQt8R08n8Y%2FFfayZ1NzxQCOSzDzA3gX162dnzgFzZncgcO4Plz5tihKfmZTHdGWiwHbJ0fRWQ19EVuSay94SOQVbVDAShzVx%2B%2BXOT7P%2F4%2BqlvdTkJIkjoNKDiP8bYwEZTZLba2jCbIb2U4LyEOboFvTPWdDZb8pfG7PU8sGwbFccOk0Wa2LCI9dJpuiFNxW8zKcyzatUNUPFq02GNjWeXP6DMaSBXUeb8HD5mIyAboKP0RoC%2BZ6hL4iwcqqxzs8ojpilhX0xB2DqYlwSnSuftHD%2BGbGo2YHZTQAeKC9cbppsZ7pwI5iTGTHHEnPQOutVDBLF3hxljF1cz7Nu8Lud3W3Xn2FUo%2BDTiMlR5s1iKpdF3uGleKfcqQRj3ACVNP15nuvjIiyf5tAFOuvNyiDGJFaRyYpgc8HKKuwSbC2Whqz11%2Fysp7LKsTcdX4b9LgtEEQUtfyWKdelDAWYlpaXJMt3wWAXJKgRUN7e7m5EZ0BbPYrwuHcPfSfb1AVr6H35McURoMHTgowU0XUT2bwDRMj2xc7vsPs1vZtOK4Lh0prJqA7gMQRUX1PDik6Xari9WOnuuc6JVe8yRD0hk7TDsg9vGBjqkAfK%2BmH1%2FMyGQ1qGp4uVZIH1xb%2F3%2BRGaLcKXg%2B4PKU9HdU3dm1%2BqTKwqCMHoe0fTC5KA9%2BaOpfPz9WbmXZUsRSHyu%2Ft6n1FPYynuvHnsVsJ4FqvdyVdl0x4NPdWCJgR%2FbC1HY49UdURCiOQHwoY1F3eOGlsf9WWQCoEHVHRLbsheY4FOtoG%2FBGZahNDlRd5hWa%2BBooAYr9mD1cUwONnxjMFzOaH4w&X-Amz-Signature=c00fed3e0300470da74b81a556667f0197105c6f6d9f390f5010f0c3af6e88b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

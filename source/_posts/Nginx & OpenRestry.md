---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTODG5D%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpVZgx2XxlX%2B08fHEhlk83medYXiVdQ%2FWQ2OTYjFtAzQIhAJ3K4wecIpkxi%2FAkTTosonoUPfYSg3jBQKhNXUG%2Fb%2FWdKv8DCE4QABoMNjM3NDIzMTgzODA1IgzGZ8fz8PFVJD9DyuEq3ANV1vvBRSbpx8nJTcwiCwiGTdu2ssYKIGY1ghOk5hOzhyDkG212mJd7PaHNMSnYTm9A9lcDejmXU5VPm3P5TVmOpKd%2Bs9kI%2B7cePyWjCgZBdaOdsQShJCVbNdsTWSpwmS3QE%2FR%2BGo16KOM1ychzMudj12jqAT1YGWwa%2FR%2Ftgxned7XIVpo5qeb84QXegmlnfu1lzZhIuNQrDkBcgoCQIwha3yWe6WKtUF8e%2BR3A33CKK%2BmC4cqNSWunEBM5fwtO%2F1elTeQaqWH1PnXpFenxitiFg7Spw1LfO7Mxnf%2BpGopoWwj6zGvh5%2F48SPCJUE6J%2BrmUfZ4Aq5C%2B9FrP4GdfcWuYXumS3BCkROOphOYj1ch%2FxNjT62fzDVDO89oxzrzU9GBEJTAGXktLWamrltmiUD8T9nD7nenxzOV74GzjHjVQC8Xx7gmVMlpfc1mt5iR53XsNYhEPuLfctEi%2FVkilvuMh%2BDIR2arjf1phthGYoUygW%2Bnbr4g4fWUAZybbcW%2Fs7WdNJuaQuxg8HOqCoQU3L5yZacpEIJw6iAhqSShgfdvAJP%2BAId4CBqfZXCCs2H7PsGHd0rrrDg%2FYX5SLotTU3ziOv%2Ff8Xv%2FAPnN40GM1u3Pg6qn5DMCh%2Bm6POjeM6TCNnszGBjqkAck5VSy3rFoju4l%2BMPqk6At42%2Fr%2BsHQkZRqyTWQcm0UYwvh21Wf9rN4Oi%2Bflftjbkg1fQY%2BDLKJrH7tk443%2FTH3y4HJ9mk6jrgmp6AEPxRYXYqyRrTDInZGp%2B396uQs%2FHMkcDtmIAvJex5qPMudmNzsodrDqBcvCRa6v4kqch6dXWOqoInF2%2BvzQ%2F1UQcupyZ%2Bl8BIkQkBqUzhsyEKFue9IbiWR9&X-Amz-Signature=aba51e9361e58b37205f5b52715582e1ab58820130e4986a6b4011a981e15edc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

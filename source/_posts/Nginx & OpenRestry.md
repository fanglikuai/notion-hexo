---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNNB24L6%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCh4iLjdZsgRMm4fBB0YgIh4SWTjZhQlOFjr3N43FC7EgIhANlwG6%2Bs8AwJ%2FsmGy9gy2M6c4fVpxBa%2F%2Fust7conLMvGKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxOMFy2wJeaHmLZ4E4q3AOGRkcS2k5FZi41ygPHffSQOTP0c7t%2F9jXzFVfMYkN4evFVjOL1V%2BK%2BJK7LM14rGDntamdhJcQMgeTVsjZ2bS7UK23O7GdsRoP6O2iMBfq8ZaRf0Y5WfSGjBB5Vuf%2BhUwa8%2F6oZM5pE3FCP0eeOpPv6VC7u3IotP%2BBCw1uacBR2x5ZuaiT330FcC33PmK6IPorT3c6PZaJhrSSIqCjFud%2B8pBLxYXvdvSkZlRbPHZ9hsmm%2BWj%2F9SSS3DgaSiwPtbqhl6D5gI6Mz6h1Fh%2F2jIWl0e2VJswcibq5CzKTuoI0iVmiPKajpUODn3bsA2wpympD%2Bq67kQPtmOkUjtEczh4Lr7UIBeCq%2FA1X5lEiKYSYC22dRqHIAlD4nYWPmwteKSRqp6j4teKcm%2FgHSydxwV%2B6MynMduScKViURZxAVWAQmR70MU8xwMTiTSW7d9YM9UPTly7xeOPO3xiAX7ldn9KBCpHFzWei7c9APiNEmZG602z%2Bzwbw%2FYsMi1TeJt5AdgxAHMWvTT%2F4xvedlSxx1VeMq2D6zMHoO6GFmt01sVJr4YJgk7Ep8DhoGYcacO9EXYwv0mS6eCfFGzoIWbyP%2Bne0LSwA3HzXD0vjyOACr9WElUYpquJrpu2xN9UMusTDuq5DHBjqkAWJOH37pMELjJnANrOdtPvDvqmuh%2BbbSxFUVY8GTWY0lCR7vKB0q4szzHOm5JChmFwRloJ2mL6gjeH9UJlZW6eMsDi7iRt3WQf7WtvL%2FqvWizlpXZHm%2B4Bs6pLyWmb3JCW6QKRW9GYXxKxlZXaEMp%2BT1vzQzGzL4MvX3Lif071c5SZA6SFprGEyq0Qs8DyvMneIg03lrkzS0eTObdeet4i40v6ab&X-Amz-Signature=9c21260cabdeedcd89be190007ab5161e8b7a9b67d2a127fabbb9362fabfb779&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PVMD6FV%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQCQY6imCtuQioLiAM8S275qQMs%2FFI4Fc4F6D3YHiQQnbAIhAI8nlO9JdtneCnI7Pr4VIJDdP6KS8sKmYpl%2FMYDL6M8BKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxivMYd7uc3uRwAYw8q3APIaAhaEbui3Loi3goUfJVprd%2FHfiIqfNzDeAlSD2Lvby7e5JOmynZGiTh97CNlB8RrCb4vE1KVSK%2BcNpE00aLbgKXhBw1TVn0i9YXo8fUtD7y6V0O2W2y%2B1icgsu%2FmAzpik6RgaJNwHd8UADVfOoJqIcQgHRMdToUP2JJOTasqesnkAdfG5r8hL1jrISkHC11agafrnLaTX5DnQR0%2FZBzPLprluQLkJAnR4oUAsjpOxXnRZH4waOQBJkdFfCFJB2JPwXDnzj4CPOaijsLkm8Cj0YQFpNHYSBmnQ4nasdPTfaJMEM7ilXeMI9C3LCDMgWEltw2RNh7EKjDuWBS%2BV78gti82lsl8aIK663ZLNbHuEOIZGyXf0bOxlt6fGn9Xw3ptiGmQFX4ULAzjpOXq8BzalqiEibih6M6vI3xMSh2jSgInIrdS8g1eCrAR7wH4b%2FtDNpcnVKbW6tQS6B9K8YHSMmXfNHhTuwgy6Cr%2BFuWvABts%2BQ0bn1D8WYH38pFkYj0pAHhPPwW07rEJrfVuX4pCxm7gQXxej7NisVrg9dZBmDKVAN5AsHL65DDEoNBofAqnyjdGMTBHz8OFa2HHg3VhAAH1N7S58Tin%2FUpeNqg49paOlW%2FQgphQgRvzwjCzq9zHBjqkAbgjKDsNkqV5JTkcH11aQXJ05QMUOx6jmbI2sFkQ5%2BLwq%2BWGW2BmzKFZlBlNpSiKYnMrtzxLiWVNJEkQwXbDjX97wEUcN6EmYX5kH1WP8gpCRk%2FZI76ZyvBmgPWSiC2JYhzqfrPTO6HhZFiFs0BNa%2B1VIcGZTh6dI%2BZ2jG7Z%2FEIGPl0xXgA5HIHjkVbiV9MCuaYCvxYePQX5qEOnGyR12dErGZfA&X-Amz-Signature=46377c24217130fb3c28aaf49956d36b5f33df86f0f0c1b128a374915166012d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

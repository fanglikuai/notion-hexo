---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663AG7JXZQ%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDcvkWUjWd71vMqs79xzvHaHaAlzJSkbftdQmzHFqbqKAIhALBWb%2BZAVFH9YD6%2FsWVAHR8ZXjZK0MBxB5NhSS4fGARuKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhM4nfrm2EsnVPbVgq3AOSHK2MeEuA1QvNwUXS0m2IXhO%2FWlBR%2BNyunWo%2BAduNxd2shLCYOQwnN6Ize6DfxP3bT4A%2FOQFu4dUVhiDQYw0XvblfA2lKBSX5dBZT0N%2BQFeASkiMJyZt7q%2BjvgMOTHJqUn7qjsQIayHXe2EESM%2B%2F0%2BbFZKxmVU%2FyXr4xk7KuVF%2B1Dqx%2FhZhm33vdxdNSEr783483%2FVgcN3q%2BnaOtvISiKxnZkDpw7UqU0JyREFQGikn5eg2BOLuHM2Ae1WAmXW28GUOI0MBAOy7tGX9kZ0dmJXPb4%2Bgg6rrpLtA9AsNJ9hJUUHZS8uomXv2JyY6HNyq3Wrt32sjZNmf1TRrrLxg84Uj7PvcFJl%2BlYqszrEHB7JfncICF%2FfdKWrKo3ItvPKNPru4mykxU3kbhLdn8ScoX4FOpEKNtUpxW%2BFXbTVd0%2FO39ANtvzWBlAOgPkjzn7VcwEV26B4AlTBukY1l%2Fc33ayBF6wo70kUqpD2ed4pcAUY0iJ6R0%2BBnmxBCBzNEo%2FYLQUYtnUplQSfdWTGzWKRAXTR%2FYdDqjFwmk5Aiy8lbaNAgjTLLdybzff1G78NqPPi0FyOYxgnO5ki7cSGEAN9dtHEc2oaAxwpuXmCA6L93iqNZMpR5Cy80vEKdgomzDasIHIBjqkAX0gAfVsbmvvvyhOK2r%2F69hmyT4xkxGdv71I2TFRHlaFXF6M8m%2Fesd2RbNiN%2F0K21VefsJL%2BdM4y9M1gtrlmr2CJurw1wcbEbnXURzJfOcipQ%2FYg%2F01ryx3VhQNQHTxWDZW6jChlsvHyTnzLRSV%2FreR4GCaUNnvjKOEnm5nEgN6SjrwI%2FYVOtW9KB7u9bsQPZBksUexh77sEO1hV1t499lPmO9Zt&X-Amz-Signature=cb895c1c30ca6036eb38c9cef8754c10714898d32ef2bebfb92c757d89db5bbd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

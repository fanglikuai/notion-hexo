---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V76YQ4M5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEMBaONppG%2FPxJvoumXemSgBFjJJ7gIGmIIhH4NSII69AiEA%2Bm3mwU6Oe7Mlf8XRNiqeETF7J5dNOMeCLkl%2FV9dDmqoq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDKoPzYz2ol14n9%2FO7ircAz8VrUXAA8%2FCgt4TckUWdJ8wdl3KV18bc4EbZw483uykzGbZz6q%2FJ7UtIfEVTdQYellcoDRMmcFw%2FYjwgMwFqNKg3cYy8FjovExWtfNhn1CvA4ZNJYy9S5etOXmJ8BwJ5gsOs1sjQJGBlqxVFjMnExqIXR298E8K2IJKRByT65BE%2FzMvKvawKdFAJi%2BTV%2BwEk8JSzxQ%2Ff40%2FLwJK2tmAhZL43QMkL4XqSoWLa1su8oZ7R7Nd2MFpOMyTFI%2BWHxsYolbuBPPKWwAuZVAb6IOioIH6aDE8%2B%2FBOjUlkLpZA3jgYVkqVW5znwGc5fE3OqH3r2cfHAX9EA8JbcLdihIJQJsfxV1KJIkLaMeDUqNBxFIvMVovbBwW5JSJnd79n8cb95RkjM7Grgl1da8I6RNSjxIK2ziXbmR2WxKoaZ3QV%2BWjk4TvK9dkSsie8qGY44Q7yeqd6Mug%2FFVc6UHZ%2F5qx4v%2FuwWuRbP%2F7lCCfUG06YxN0AZv9REAO3ghQ2brV3iuLOQNoEAIIzPix2K5Anb0fCXgIulHHE38lWCAp2otK%2BUh7vCHIKyQshh5nZRBUO7PFdLhWvkD3lHCFv0ILjTBa2K47D1mXylRwovHkebtZ9gbvSN4huUMPT3lsGh2SGMO7owMYGOqUB57ZkNgckkbJNFoksFaD1sSKfv7jW%2B3XLoDbCLIwhd6flGfxHFyLHI69kRLFf%2BkDG%2B9KXeC5fkufGP%2Flr9gHyVnhrVEYO4%2F21yBMH9VV4zFkTPx7e6V2SP%2FOwsBEPsnNLUNpd2iYvWJgkI49%2F3o9daxUYul04%2BKfdg58SZNF8CO%2FYYj6tFGHHZ0h9IOhkSHWwdEMLvjOqpj00INJHtI9DxCzYATyt&X-Amz-Signature=cc2f2b68f6a32862de603967232347b448bd5dac724c1707514978f4c420e17c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664DP3UDH3%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECIaCXVzLXdlc3QtMiJGMEQCIHv1SehUpn%2FcGskqQbyKHqK1pELvH3lCzgBijIoKA4qSAiA4OBfhcoZewfOe2r2Fi%2Bc1pwG4sliiV8dM5h9yIleilSqIBAjb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXxhlgCO6kKJO0tx5KtwDom1TPOhXQarc4b1hRMlyrIjgAcAmg5a%2BqF8FI2eCO9Tz%2FR%2FqlTof4CTda75J8uq%2Bu58Z60lRcPyM5k6p3jMhCWswf38XV3vNvlH%2Bx%2BqfqNwfBVPbkJ7fhW2iwawCM0Hvi4QuOrFTbPsq4scEoROkYLTNLDbUyc2mQoR%2BTHNeQxL519D1bYO6LbB20hAS27x0Cf8BBLYsAZ4ViXa%2FITX4r0gGmu6pSiVc06rx34bQe9PAmac5ks2zDAw%2FhkqsVBcpn9VKmToJgRrodPQqT6cQJyHRv66hoKKRQb6I8JB%2BpNCO14I57VnA%2FeOpWevyQfuPvTPOX3tQjH9egG64QjjoVIvc1MQDCb2yLTVgK2L%2B%2FU3b3hoXabKA8lpDd0pVuWMI%2FLGQx38IiK%2BeC5gl7gb7b48Lxp0DQPGiM5dQeOsAySGKa5dq6Zdkmk6lP9DylJCvjrUOj%2FMANp50UldxWrIBqoZhlVB%2FmrEBScPwemufDQpG7ZdLLzFUjTDFjFGanJXgXLHmKE4ZWlF3ZdKdT1Vw8e9x40fwfcIyqhZpYgLenJFlxid4krMrZivrzRbtABaItel%2B0Ri%2F7CGNB%2F0Cf%2B8hEMuOK48tbtoJHsErVQhHZHrm6OtxZn9QUnAUwwsw5JuJyAY6pgFyaOf2H9dHhrAdcZaX%2FA6YfjHkYtCcd2ZmTJ8KoE4IYxy1mtPlfnEWT1A5HlNTK5qlEGJvnW7%2F93GvcLIz59CMlk7Z7AB9xgZH9qVnvF9Ea55lRtvyVleOc%2Fm3yP5tGttUCK%2BP4M1B0MII5Vp3HQQirc5wVhAwGJgmC5EjhUtIz%2F9GbpJ5WB%2BRpa4RHntOrlOB3ikQQQgxXztPMeMYbXBnh68a4Esr&X-Amz-Signature=05fb3f1deedd008e1b65d605eed08541f864bbf23b2a04874a060310e64c02ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

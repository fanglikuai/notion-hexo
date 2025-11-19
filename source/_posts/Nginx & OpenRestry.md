---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XJ2BW4C%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T090051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBAaCXVzLXdlc3QtMiJHMEUCIQDdYcgo3NmNGkDqG3mPol2hI8Uoc0Kb2sOVhVery09fkQIge1iOuRx7MLxHTzjhb7ricaVIBf4ybmdX%2BNEy8Cn%2BYP4qiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMBJdormG3Zf8d80rCrcA7I4TJvQ%2ByQEqqcMXo35TXn6%2B%2B0WB0C6MbmROez5nhfoj9zZvBe5DrVrmEC%2FmULc2GJUxT0gdIwGFKSZa90WfL0cheTAzoI2Z31f8Q8R99hpytLmaq4psP6PxTV%2FuOqD%2FSX5sV7y%2F1%2F%2BvlSuBIAzbagEAJrvTaS%2BKuewZQfzJji%2BQMcSOdwsgv59JuGUKw2IKOPSwSENTfEjlYIjDL8lyCjpVpuSfWHeYlkqgFQOJhq3yQZSsYowWBkG%2FgF9UkZe8PGVvBVUZON2oN4prf0pdoDblZDzM%2FQdAeR6aCUqb4FL6tvUoeVu9M4YJjgbtBAOax0iKrAsQSvMHDE1FUvufcxMbmTKH%2BpONip01JEN%2B5W%2BwPxX74FWQhEuytHPf61eVnEaX4R3aOZDSYqKUFuL7o5Q2m0YWrCLNGVEWCLTNMO0caMhh1ul8YBHPAZ5o0z5rV%2BqCrZV83VfBM3BbCFHQkSPLTl8P%2FYnYt3o2wDERbvM8doZHgE6VqZ0pGigy4S0UO%2F1vIBpZwcnKCfwiDCYffSos1%2FUDvK1ajVEXYt6VOW6DFwndwoYh9efeJ7PAPJkg6NQR%2FjIFH44jr9gg4ZZKt3a3f%2FW5uKVl62ix70RWR6JAi6FQlrx5WBnQuknMPPy9cgGOqUB0jMhm6S3ZAVAR8U9bAYS%2FcIR9DsLdJ%2BZNMkCSwSRLz4%2Fh%2BnWAP9IHlSNgb98Cw1I5PWwG2ikAg%2BTobJsFoHY%2FB%2BJajx%2BNfTTUQoxZVKxsQ1I9qFyhalw1JS%2B16OeSRQ7lKiYrZzgU4%2FDKfDSSq84g%2BI1R2h5Wy2BRw2mRccuUXIkzA7doquWw5unWDKzhpZG58geX5ZVKBsKTJLITJKOaej5qlOB&X-Amz-Signature=6f6b2e515ef01d9acc946d44235cdc58cdc7c991967312a5cb67ebb70a8cab33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

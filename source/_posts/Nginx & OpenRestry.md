---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664H5DZRHN%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T140108Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDsUe4Wq5L3KNseRALcl8mx7%2B6GAroBVRtETM98x9%2B8BAIgWM03u9k0fObZveUprtRiCQvFvXpI7ZrQetHQZcCzBZ8q%2FwMIRhAAGgw2Mzc0MjMxODM4MDUiDJsRuApzec8547zWPCrcA3%2Bj%2BenySulUsaLvSkmT29IVJ5OEI8MWMr9csG99g91CW8cXW1zsuRjMB8izUvkqHL9Bg2boJR7PNQ9b8yg05q3RBfbl3QkmT8v5s82F%2Bd02QvOj1UJHc1IQm3cW%2F05Kl1r0lVQFxWq3Rb4jTBzd%2FsF6dRvFORgTJ3x%2BaDxXOO8cMvR9AblAMF%2FmDAE44f8xIYuod2C7VU8IrIdQXC8EcQtltwPoL7n2cXRSPOou2EmXG%2F%2BAb2556doH5qA9LLt6yDWa0UmGWn4hlIpGW8hRD%2BPDQLPeMmtbEZnSm6p6dBF6GU1g94gGFQpcO%2BDLqe7maixvbZHTwXh0%2FZ8K8vmlIpxdkHmyecAa32k6IPzcU7fYiJxQYccXpck8KYNEKKtWjHwyTMEHycE2oTVj26J29gTo6blJVY%2Bx0YgojqWOPOI9feefd4TgZlUvDk%2FwJPodZgua4ZFfEAerz8E3UGCWDgMDERra8huH4fONjprTg8KvnuWpVg5UvM7Yjc0FGCkAFH5gqoUob%2FHtZzQ%2FeMWjj2EhXMsBv7lyA4IMLa%2FdIIMhLLDm8t2RGfABzMI0L6Lu4aO9mJ%2FgWX13GVQJdqp1HFhuNuzgtdPxXnOBPxQsgOF8nRyqd7VdHPbxr%2FcUMJDvs8cGOqUBjEoby3Z5da0syqbEu%2FsYZ09%2B2SpLwgBH10YRUYFWJi6%2BOPGo6Ggz1MLbDZ7GnyS6GbwdlEbFc8qo0I4P07iSD9BwJt%2FFaLXN9m1jiaixxbgRWgq%2Bx9o0SzUCm9teD%2Fmjd0Dn55%2BWA3e5DOmYeRjheRqm8oXQK2JS3i9H29kNLF8PCm0nq4gCwO9RsqN6jVaY40a2YVKAwH2iDQtpKMWUQQpDgJ5s&X-Amz-Signature=f90b0d27ff5de7fdfb91074f74cea941a3b1971f0181de5273dcf411356eb16c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

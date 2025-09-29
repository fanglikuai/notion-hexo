---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662VNVB2LF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJGMEQCIAxN8QUbT4tUSZLlEj0%2F8SvFbOgoTwmz6TwIB%2BlWAaeQAiBvEjCwYzeJdYn7oUWgWv%2FXoqf7hrjXZ3LuoI1VS2HN9SqIBAjK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSyfkMOupWglOdat6KtwDg0lfrztDEYzd3QWDnNwHJIkwiyANWXoG6phmIGJf0beehrL8ZpQ8rkO08%2FND5fw%2Fl%2FqOPkYssG18cIP2nW6KzT8tXJ5nwVYxPMBpj%2B0lPKqvTH%2BS%2FPcNIJOjCDvPAl00tVvWGoqwvW4iBtA19XbAqlj%2B9oYRlXZReSB%2B3I97hdgL1KcdcDuSBCLV3H2C2COcv5uYA8egqZcfAcTIXQgi0%2BENMLimNmT9IEIRHZK6tfbjItSV7J15DA5OnYKPmVetTfjanQpqff6jAZ0UACfLIgL2YfUBQa0Ir9EnyuNUmlX3eJ6TpVv0fNZbS%2FoDioK1uA%2BL3uePb1rXeDlUcK3FYlWts0vPVkbxM0GYR%2B7wypCojkvCFrUw4Q9YJKkPsBWFkkB3Xs7T1JqQ6LFVHzClBcHuLfxrGMRBETOnASWje8VakSHZSz07gW0B0OyCklYXUTyGeQ7FoxF4v9Vlkb8FwHicqsGaA75u1ZToeKduhjzYM9QV7P6MwYLnpWpc%2BvSOZLqHmwW65vjRvViPnKcYNSBeQbt0vh17bQcDDlnfQMaN6B7Jr229j7tI6JN2vgfrHDo9kqF4oYlJQS7lmBYBoqk0NzUFdGlPqNKuDvYTzg7b%2ByrhlAZ8tXk44pUwgKvnxgY6pgGYcCgwt0yQPB5uR6anlg8HNjbMTrg5qcaZZwjbC%2FmJspJuGcZ%2FSllA8WOU9xAX5xPabmmjm87nTppDpTvbkac%2B3r0QoQIT%2BzUh3zEv77K8aVPXSlHYuhdFHtSgyFZvVhrSOkiPDEjVEygg3lIGHl%2BBtOLWmGKLDQ%2F6SNK8PFINyrMErCbeiG1xhSbrgQT%2F4UG%2FQIBOXCZ3LJ8w%2FM2i3t0UD%2BS3afvt&X-Amz-Signature=8fc2920d321d62a929c8d3d88b97d7b0ae91e1a2e2e67d4f09a96087e306c872&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

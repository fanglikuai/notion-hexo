---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VJUQRME%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T090101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJIMEYCIQCC6frgrB2IX9WPpkwezsMWwm02cX9noyU5jLGFQmMb2wIhAOa6Lm2H2dnHYiSRLYPN3NY%2BXLRGfoSybCD1WFQ0veKiKv8DCBIQABoMNjM3NDIzMTgzODA1IgwRK7bE8i7m2Jep9Ogq3AM5fyHBx%2F74QdnLYH2oK8DnN5N6xgVDgL2GdzvF%2F88LooDq%2BSClv%2BE5L4DyUh1MCNREgZfQuymIVzcxdigd%2FjEx2qDLwpTAL7LLNzhXQw3aOK%2B7SXQkJIUNE5piNMz8xXbJKIocsECMto7ct6r6r7BPggOmZdi5TgubQt28ToZxy1lGwvOSAR1rxXQ7KrZkIbdw34UOVRrf65izZVMtPMbfxNMPKsLrZB9uXBfmZG%2BRL9HGHQhwFYH%2F8YXFYQ1sZKzRKklYZhufLqYlMMskwCEIu%2B9OVX2LYwhiV5q5JHLyL6VZ0boc5RcYhUa4pfW3qFTYXJwYnZUI249ZUBIpFPBGPMz0kyUm9bvcMRodsIc9gPwg31MWDaAuZbTQOff9cE1y7El%2FOhCMLvLlj4IWP9iwJLTfrlBzGosKQ1SG4ChJA7MW56v2hABRU%2FunVIu1CtxOMQjOq7MQsxjY%2FBu6H2r0W%2FfI5lvaXPekMoqserJG%2BM%2B9w2hxMLA7ZSp9Aj30KDG0JMftQUHH2uQEZR%2Fq8mW1VcYUkYYbeP7vtVqqRY2OnYGdNYwjdesf80mFpAhgoaES1HOaNTM0wx8qHgYHJ5gqJbSHlDzwUfv4TdxXfxfSaT4NJmLQYoMVsN%2F%2FqTCElN3HBjqkAX5zdPPWGCEDXkpcTOugkB%2BGeum%2Fa0%2Fgf7xjdRwL2SfPtroJDxdqtP8mct6uIa0ufou1RonoWZwXiF1OuFnr4NXVd2%2BMSP4KBsHhAcQN14%2FQNiN4rRnx3YlMftC2uCi3kxzv7BRz9I0Pt4w7JETouYVh38QSyJv41%2BrQyu3s990lLsIH8cCsW%2FNCnOz0u7OtiXsN6zTifjq65cnKC4TZRbwzgUgC&X-Amz-Signature=4b63403556441660dd619d91deb746e078aa9a04471ad7342ad584fb925366fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

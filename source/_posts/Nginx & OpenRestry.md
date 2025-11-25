---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SYR7FWER%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T100104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHcMrbjshJU%2BpjCn78HBNndRWaG0K4uidiCKwn8tKS%2FWAiEA8YGHNTJwD7%2FChel8g%2FmS8dPg%2Fp4jyqQcgBRmYeG8q5cq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDC2V5esG6cy%2Bvmz7tircAxGVxT4nkgbPTaqyAXpWykj16yJOJAUuuSWbsfEuT2HEgnxuYVRMhKFpd6U%2BK9N3j4AywyZIq2G7Uqxztl3dbFtU2GwDzPSzcNbyZbnCX%2BB%2FOEKGtopRsNbE9z9ap2%2Ff4al9FmBMQ8y%2BjagHzWe%2FF%2FXATsmfewhyGlC1KTih3iQeiWWxDHMO3oTiw8My2mvpYI7%2FIUHK73Lnkwn2kSk0jMAxmCQjfjlTB4UuEMGOgz5jgiSt061A0r8bAhtEiuxMHdMMqugSMtzGKYkhG5qQuws6qEioKjQLBkzUuhA8eJc25R%2FpTNVfIwDqxkKSQTSmva41u%2BcS7Rh9c%2BxJa2rnagNdkxrxgxNUZel4LB0sL2V6geFqoMwSvp79pBTBvNAEdVua8EMrHAGalGWJzqeOTqTJJtW84ABLPWnyjaJUDt9UJsxvzsTF%2FgUgUb4lygdYCsPobx3TNADtNcNdToOuaGump1DJCY24UyWz0wZCziqccLL9YQBFXlZxVlmyEnTeg4c3Qx2qb2wm7BpFkmTD813dg%2BW9BrSPVxOr15WcTc0RCRQXWi6y6r35a8jGhnYdTL0xA5eRhCqxGWuhB3kpIKe82rltaB1VoIBr60AmHXwleMqutSo2IIffgFiDMPrZlckGOqUBktChLWl%2BzaDEOrs%2FJJZeJpHpPbfx1aPdJt2o6EPipYQWfrmhIpHul4q%2BD3040Bff5Ml4DSCDeTa2wugnoMD%2F%2FSPQg0oBk6N8k8WvBOQ08TD%2Br%2F5aozFl5Wn8G8Covs1JlUr1uedHISRgWNu%2B3tAKPyLxTfTeDDpTj3%2BgT%2BWWa0lRqCZyLhc%2B%2FnVFOKHeHiNb6n7eV5pzPExdO9Dds%2Bs%2BZmyy8I7B&X-Amz-Signature=69809f7a54ef4b5c4831e84e78f1361305657110a18a2884f8d1d9534399b937&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

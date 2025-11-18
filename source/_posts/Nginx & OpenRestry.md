---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664I45ORPH%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDhprBeyNA%2Bbv2319r%2FaPL6pR0ee8%2BnTx33dlP%2BNv2JpgIhAKqnwTKJ91MXVYpdksEfH8jgnZZqMzmU%2BTe5BIPCcZNcKogECMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxMk%2Bij14iOCcs9c8cq3AMmMZug4VRimcfRW4egtFz%2BjJx6a0mkYrKIcnmIG9to8vKpMBR22w2vEdxXHav9gcvP%2B1lDNpdnyIpokrVjh6z3IQeYh8weqElHE1k07%2Bbj1rX%2Fc72XpEqQ1lrcFTbjfUCD688qe0ppDN4s7ABh5dJRfTgW3Gp%2BpLygCMrrGbhf%2BJMUBlIodPcDmQLiN09Fiu9aqFWInv9jGlG%2B%2FuasqO3WjiI%2Bl5p8BjpMf5OQHzzHAoQam3BmUUvW9v8fpXGEJeYJOXiZbu%2FgLEiZ73Oaitatc1xjWd4rVB%2FiUlt%2B4tigb%2BjT8s6Bx6OZIL6t2AU5Q0%2FELDbCM0iiIh9%2Blx9e1WlbsupBOe79k%2FZvcKZ53XdRpNQ8UQLO7UcI6%2FfFMEk7CKgyu6O05j0EBm%2FKUADEzmOOJAkGozfFevcDvnK7m5ET6VErYfNE2GjXFL2swu4wceS%2FmB3S3My%2FcSqxzu%2FkN3DGTO0xjG3%2FlP13Aoo4g796iMauA0dVlwgCmqg11mBedANmGPZYaHcn6htkZD2sbqgsXNS%2B6G1coFP%2FRNyx66tffF1Z5N85LSYruvb7ZL5F0TM7o5i%2BfnxYemuVY2jejnNH1vZHGHCJ0tgMN7Z69%2FBHNFYJ4PRQ6FQ9rNIAwjCOxfHIBjqkAXk8IVqfh4hAOsGIRDsDT5AAnM5bFv3gkrqXyzOPwJ0X6AOP4hJ11J2eAlrSeakXNq68FKN%2FFWxy%2Bkn2EbeWp9Q%2FSTmFLJlz8ZfJzG7qH4FxBixoSJs%2FqSQQenMBIVCqT%2FmxOb7TDTqkOSt90sC9s3k0o00WLruNlMCe%2BupnRfg3e7v1a9SKiTq%2BJ3rBVf1L1CAOQIh14yNQ7MMxZdU4hfmRLoy2&X-Amz-Signature=3508d1993a58cb6fd2b8a8154b05cfa7229c8f2db4b5130f95c700247ad37206&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

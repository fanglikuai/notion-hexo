---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677K5X3FP%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQCjpyeEOm2PenfHuzYyWn2EIAlIhVtY4r%2B%2F2JaBHUjAtwIhAN9lmLfSzDcijHrRt3M5DRQKFJWX%2B2pFYP26kXqBsOsXKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwnkS5M2ajd%2F%2BxqqNoq3AN2OjknvQhuTXfoymWXdODXvSEjD0QzUqi%2BwHqUbWwkIKyyEWdkzyp5aZXctqAxbkrR44kYFx9IGNPB5hh5ZHosxevVdqtU4D8e3KAp%2F4IqO0eNxGxPES9Ry%2BIpW1jI%2By568YcIN28ardfJN9INkZogZaxM8MuzJacTYtFoISvMdYYPdQ0slglSCFeXYdSbmmJX4FNq%2B54ck1GYgmsReu%2BskGlOs%2F%2FtPcMzQqkER%2BRcpmSFOe7ZSbJidv8XcBTdl%2BWLkf%2FF81QOLpBMCDkySlvU%2FcNuIjt5dnC%2FbqIyLOZJjkL%2BAtgCCPQINU58LvUx%2BrwuKmQl8aA0Rxy7078od27KVpbSA0bOgDblp5LDAKxEtnHgiKVyQkrB%2FC46zty%2BwvTqtRZ2JAmWdZDulxFdtIBUw1Pf8fADucwZuc0P2aDp63Abp7dmPug%2BGjPEfCFGOq672GDfygbS%2BnB2aRxjs6bsRZKcUSbZGW%2FN7CJihrHcx8xDd%2B7w05so1NBI0SZjz%2BZVhQ85t5HMkDTCW7fn9olCUc4BIbb0Vh56XyfHSgjIEkEOS2oESqkUf7QT48Lm%2BxaJLvtfIkcskey3f1Wfwo6G8XEe1VcCVB16vvH%2FDSM1SAvm8KGM3PLKQkXIGjDi9ozIBjqkAW0iZhzj%2B7Ci0WyU1BL3nhGvptIaOQFnLMHCacx0fgn29Mr36yTUak8Psk%2FzF5%2Bilnhj%2FPs%2F4xQE%2BnVW3W%2BE2atgkCsV6mUMYifUTPLt0AjbI6hLBYio%2FkEE4lvo%2FtYdUmVKW541b5bTZsYQq1U%2BqhiQkbX%2FoqkkJo2vmjLrCJkoxTCK%2Fdfo1mY45ejCcJesK88ekSWDpgIKUDdw20HOVnGQNoJK&X-Amz-Signature=7a9aeca557f2f341b6e551975eadf7c44ceec700c61cc076fcf24e0fd854ae42&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

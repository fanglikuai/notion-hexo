---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KZIM24W%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T140056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJIMEYCIQC2cVIuLw%2FQVsp7JMG6OGB13H3BKgK3hJD%2FFopL5K2EogIhAJoCyfs8AE72cEyW%2B1YdouBptzY9q1UAIIMOqEzlh1JtKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxfPXI69cZBDPSabpEq3AOWGM2iEUzL36rSD9jqpvk7RG94y9uFdvsLxMsCI3ot8g4psLBaQ%2BUrFBeAxpj%2FmXgSXkK9nNWX9EQU9sIjsqh8q9%2FU5MMz0JGk8vovFGWCIiVCqN5v1dGbxsTY7pO7ChJC04wkVif6uwMshGyWavcREUNmEsowsAFBk5plildcj%2B2v1AKRsnSIWfleF0MAtrn8SBq3aMVRE0TDspA2uPJJvITQ4dBcUQxOqz87BDGW3gsq3cVtZvF7Oekvwmageur1ak5BEPAhh8s6rlypZKiMLumOvocL%2B4BnK2gV3Dml0V6rHyh6AWhiLGIqkfBYbzc%2FbUf69%2BdaSDZN0tI89Sv%2FL0mBzZDoubNmi53qgKZ62iX9nGpl%2BGxD6ijPlSCVTlA%2B0sdEWOeYi4s%2FuKdfPOSlrXJMdqrLnExPA0nJMx4xX1rxEMyck%2FxLM3DMQoZenMxC2tDUT0Rd3fxaou%2BrPvzitkfswMF3CNrir8IXHeHDgrkeWPBXNKUwyYonoSXca92TnI%2BhPp%2Fn1ghntusyqVvRgHB8o34NWs86XcAj8aOUwCAyxOFC%2Bau01GbADrwx1IaJ9%2BvCbAqck9MeNKAYrsmJ6%2BoXvEJnmW5ntzNg8R%2B1K%2B1cMTdAbTowPYWePTC0jbzIBjqkAfV5MBcBo81yMYNtbB849J8ctYBF0ojeGqcXv4grzuVGW0A%2BgqWY5w9PvsmDK5YsLhfSMVuEtMq9ZP3tOP48N4Bl6TS4IRIniGx9QOHc0seeszs%2B4UnTbE3vRApr2iFJXu8fk9iRLLrxY2e%2BNmC9sUJH%2BbBpX14sy9Sv%2BHJCaKG%2Fuod1GIjyyp3aJuKAsnr5J5CyqycDa1%2BtDTex4FTRDzlwV6GJ&X-Amz-Signature=3b21e3d5b0eeadfbd6d9aa2090f63ef49f95ed971d89a199811ee52e604480bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

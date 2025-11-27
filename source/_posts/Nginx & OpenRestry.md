---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZI4KXZZR%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJFMEMCHzOwQyS4SIQQhsdpnFAK7ZFTiiNX9LoW%2BpT35wqblRUCIBMigfHrQxq1rdActk3vKKM0vEFqOR1PrGdDo6FoXQQhKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzgmXh4YZg6UDVJwIgq3AOLK731TxBEf6N8NzCGeDwmmItulXpYxCtyBiCMxvOd5LCY189loQFzr%2FCw0wlOtQwwN%2BOtcChMWGb3m%2FAFXkKJ7G15WQjHR2IpXWYLBY%2Bq9eapZq9M40PjmwTpNa8%2FcjaghqD3uRz5ql6fhThbaoDLOCu5zzY4%2BjnQQurgYiriWGd%2B7XkgfcMu6a0H7hPHPRmh73GYPf49b8KewbTLRYXo38OVOtTcdPLdA61i7B5O0qXMZMOWFYwDTztjcMcQxJITlHX2ra%2B8klpZA%2FNPomP3lHlr099alYwM3ea3p1Px0dvoKnvPHs%2BS%2Fotu7fy%2F9rYha%2BrsQZfGvOhakQ%2Fdk8vNqKjWQZ8wmbwCE5kZE7ce8Wutdn4%2B1xdj5j%2Ft6SaeuEUbHtArSOYx8HukzS8BtwRFUjBmKBQEIO3VRdWuOdeE1Hax%2B5GwIygl1qv3MAXICxCpgXaZtz0LzdGQ98B0TZRJyPc5s9VjT%2FbY1jsIBvj51Kyu4AU%2BTD8aWbCP0PDrBLf%2BFBz7dj5Q09EJsK9qoFQQGgWgeYi8qBK5NIzB%2B09qDW9YyWNm21uPsXI1ZTh47k00jlpYv%2FVSRbgZKNhUoD6u1KZZ3e4whmPk%2BV%2FZDjBjPLyyzECw0AKN0egrczDro6DJBjqnATQo6Cuvb5iMQQuckhbud3M9H0Qb%2Bah%2BbOM%2B5EG8ZDKOPt0OikGD7SOni4xlXziIHix2ESJkgs92J%2FLrXZAEmbb%2Bc0Azm0E2%2FEdS2XaP9iRcgJGEhgCHQ0R6W4VTZBTPJ10gM%2FFJBV%2Ft%2Bmdxqd1hktgLXMHoin2XIaghYObMGXtTxClQianq9DcMloIRzCxnKlXk2Gm%2B3RBJTGQW0toMymcOutCHagSK&X-Amz-Signature=5baf3c5d500bfe2579dbca99f9e6c71052aa3b4a71b6afb392a304a7ac55f104&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

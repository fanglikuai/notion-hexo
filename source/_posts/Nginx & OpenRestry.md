---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDB6DANY%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCF9rY8SENEtlX0NRGrKS4lJm9kNhOBl7vODMXcBDUcBwIgWViHW7nLk4SL%2FE9lZjUXCBHcWHH2xxyyL5eEtHrL9hsq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJy1ZmuZVj3Weg9dQircA8ajgoeNjO3cLTww2b%2FBKtgcgmK2z8xLrl6J73chyXhlcRihhB3BJvXmR%2F%2FGCTuRUoo8JOkYAMUzfq%2F2mMW501hJkyEAQtwSuCPi3OBE%2BIIYNP2sFaNfQ%2BdesLjvGGGMIH1PVuCz1LD63qG7hJzEwpEETHq2C%2BefcjRQUqOHkJxkG%2FA6mkmk0lRUZHflt0AaMMma%2BenffoLYXt0Wd%2FoTJy%2FB6xM%2FkNKur0vCjE40I5Hsu9UWsHmbLlYUtuuw0G1MY6fhrWqjI5JtuuOPf41VUERGHNSLrfYT9yQsJly7vSsASNC68HVWpTumAf3Ns%2BCxTc3jUXu8pYZMMehfNsf5anyoToV%2BS8GR%2FLLBW0QRQiURDAWqgPYe4CmwZdd%2FsvzYLLbR6bu%2BAxXbhN%2B2TOWDABqNdY0QRbCe%2FcjMvEPAI9Z1P2kUwpTMX1ysslruiPwYNYbOJHM4vHYFa%2BAZ3ruLoxJhTj1L34t%2BW2f5KLadi2jKC7cWT35MkaxNocGGqFOHNRLaF6z%2BK%2Fi0yYtqoHrK10L%2Fm4TR5yx1jozK1CI9NmCWgj%2FSI30peW0vjU%2FxgNuWOYRmOy2vHclx4QgZPLwqbmmcb%2BBdXw%2BYlfmlD8ZP%2FrcgQiYedgZdKK%2BqlQAxMN%2BWi8kGOqUBhe%2BcvfTzReIxPdNxXTGM%2FikHiPNlPHYpWnn5EVUcVm2KNHTYniHzUmolrYPR%2BmhsJEA0dHLab544Nxw1KeE4LNFTNUTNm1JX%2Bag1J5AbCnN5rBGLWwbcCqOG5RQ4XI3rcpt%2BESLbweAj9S8OcS0LZG%2BsZ3lTIVIrK%2FcSZA6YfYxSn956YdBYNx1C3rU%2BlP9WbGiqwKPkgEk2pN7ye07KJCE2UDAZ&X-Amz-Signature=76594e93815d6a0b419b1642a6e75bb29291ca35f142ea5c75231f4579776c8d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

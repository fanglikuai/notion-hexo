---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667TIKJQRX%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAWXiLLrGPpWDHRIes9vK6OvZhGZyjhWjVDqU7BqiyefAiAyc10NWjqy7a3eRWwwu9IALZ5E%2BNs%2BjkoMYmZ4epL22Cr%2FAwg%2FEAAaDDYzNzQyMzE4MzgwNSIMSXsvi%2B%2B7tzBzXcLXKtwDocMtNgaG42lOmnQPx8lqbpWEkOPcfK1mCrB5h1O5pM0GjdecTLLSDO4uLdC1%2FOlNO3ShSlkKKCacorvdxIODx%2Fk5ZXXzM1wJY9drMKynJJ7QVxBTg9rb%2F1njd%2BIofBjH0JrDlDrC26JfcbUb11ulGoI49jACJHy%2FzUvQggZxk8clec2Z7q1TlHIFlLumcshoYRd2etS8PnRUfori1Sx8veXPwGRY7g1ZhUHo3ke8DyGGhVVAypX2okO%2F75YKUCfSaG5vx3WidfEqMHqiEnFawNSAG33yhlPBifCoE1BhW0yREEwyCUmDz66oSbVXwCYNrbi1%2BoMiGtcC2Azd0%2BGFThUD1eK8%2Fl2z2IIKeJTm1k5d0L%2Ft8tnwMXW4UjQbyR4lMOSz5Xh7GXLFx1cbg4yBh%2BR1PYvNwjYBYdlOzQUlRz3YqPb%2F3yJH4k4KrJkZZn6mDO6RfEB27E%2F6I%2B9F9v4Vr7712Qt2q%2FspS1mfGuHUCNq6EavLPv2Z8PfYOl2jIlzdFtsvQsLmiH5iTGZVil0Pazlk2qXOkNWzrfazDaMZgIvzKfnsQPuSczJmfxeudPnFIPWRmipbHNRD1ez2oOiPsFBVbcjYLjNdmFbZuXwVPWc00c98p1GN9G0%2F5R0whIHnxwY6pgGcyGN1x6P41LWLoc9N7mNhTB%2FY3tL1Ro00ac9Pd8ByOr%2FmZUIlAeyGr6DAj0QJxfn3UGPZ5hhQkzz1HkUJNS1G3tytVbWlUGh50jQ5ktjQ4a4njB%2FnImM6%2Bq6GWQDgM3bxKI8kccW8bdhd9oCpHSf%2FA0ImXpQIskLAsy2f%2FRUMh55tSDyX69jEcM5MTnabWVO21yCgvNsQ7A2cZmFM2eoriIFHYHdW&X-Amz-Signature=c6f3caebeb66de1b342cb1aa4dba88bca4449e0e993cc588beb9edf01bfc6749&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

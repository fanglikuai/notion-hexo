---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUG4XCCF%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJGMEQCIB4cBUcrJn%2BbpAKyoy80ofy2TKGmSnLq1KPqc71AwgFIAiB6XHgHpNVqTfG1JpIkb9623XRD1BOEzcgwVJUt04KAPiqIBAjS%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMDTVaYNEtgAWv60qgKtwD07Lhcj42XF%2Bt41WkoT5MJbzPg9zfxFh0buFcSCqFQjj9NQ62GXBNqVSLEDt7HSs%2Bq1PFBniIdFDVA2H95yqB8SSvuCmrRoP0gf56XrhXkRgU8Y3NAjrrKsQHefGlGic2cBHovrkCdFjKA8lh%2FpZI%2B2NnkXHVjSP1lxbHR1nQP7ShPbvUXCgpFieBSD%2BqtKaFzl3A06CffDL7OMzawHFpWUR5%2B%2FdqWhtDd6M8KIWQGhIR1X%2F7k1%2BqGo6AI9t5WJW0D%2BFiKWEQwJ9SVAz652d4ntcuKUDiOg2eOCLPOIHZCABn3iFs6fPXiXS513p3gFK%2BvpSW31aCd7nTVwuf56hUyO81f3klBD2aC%2BIVMuQja4Ae8GhikW8RXt%2FaqqO6mrLJTU3CySixGNFf4367XfaTEfnCSs1A8LCqO3ywmGHkUTLxcl%2B%2FrYj8e2Vo3GATbWRpcLDrgth11JSoz3w31xJ2NK%2F9cjPrESataadPX7BPLHLnpNwTILnrlwVoW%2FqfvLEQcdhvFx9qepUKEm4v01yJY3YWYTL09ZDfG1v73uEQQkeLY4oNpZwK2UB%2BW1W00u64X%2Fx9D48Ljxc6HTuK6zimFov2nEr8tRKQOsTqyXd%2Ft%2B8YkfPHy8i7gQaXb2cw7JHpxgY6pgF17G%2B0D9czaHOtMRSavywcb%2B2aWgRQXFeP0Gfrj2E65mhrFkmLzezAf9%2FxRsDQZ5wRk5zncaQA9SP9Yr46eyluu6BaIzgam%2FZXZ3yIt3ug1T3xXOuvL0Ud%2BeWF0d77z6T%2FlNx8mvYSt7h%2BHkDR%2BPO8VpCLrHb1PBKeb1bUybSEmqK1zq837BO4nwxk53LaryeVv1zqOz51zTUsTOLqqvmv5G5wWeDg&X-Amz-Signature=7a43abf65eee6bd9b1386c4a7453e563e6273cd0fa07be586c6abeab3707d970&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

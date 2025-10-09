---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VHHOA3FS%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T060100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDIRUB8nYZYEyKRVZ7HFGfRArB77kGD5WoLnjqX13zdogIgJ3UNSqiA8a68DNYoadkZIzO8zrJctQwV4ClO3%2FjfAmMqiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDESWESoSG8%2BGS%2B8hBCrcA1U4Pvq5YYUdmRRxaDXDHQk50IWcleLObckLL1%2FP3ewqKPM5hcq3ji4jHypSQnSQZFF67z2g%2Bxp%2B5ckTxn1Raj9khTQM89SCmUnq%2FKgJQk2q%2BBMokuewy5lKJWp%2FRiNzh8nLJ%2Bq%2BjLNeHPGgcKIdQuJkQGuou6u0se9yBJj%2FtHKkt12tlF0EMqi4C3xe8qKtoug8J%2BhipiVn1%2BozE42%2F0d6MmeKqzD91T6%2BCtt%2FKdEWeolProFDKUEvIwulcEFfmzDnLnQBKaIf1M9LVvOCv3uQuaY2VOajVqjCftg7oJi1gy5XTru3M7BmPzzV%2BO1e78t3pzOKPDgfOVfUCaa8RoMcqC7TEbwH%2BerE95FN99%2FCVzku5EwOG6EOYKt%2BG7xdMpMSuHNElextxRFPZtG35lGThZ96TqSmoVy5U4vDd%2FfYO9lKTq8Yg6dlZ5%2BGb2oeKEVZkdFgddUEGKVS8kTzJsFjrNrcTuE%2BDwIiPLYpha6bXdA%2BPkIp0utuXbQzBCDXckNCi3UbljQZHjJXbRdzgZj8jNQfj9t7%2FLj%2Fy9WtJkXG6Mag52aoWd0ZK89i74q86zrfgafAA1eJIDP3u2%2B2Rn7NT872YNAMYtEvhT9AZRdn%2BGg1lfDY0AFpM4jhOMLycnccGOqUBOOvS5UFSTebw1GCuZJoXFzDBEWBZEIVoQYK9HKIpkT25jxJzv4NYxwSsjMd8qCUfxW4NxjVmS3K0EfN7HKiR1V4Ko%2F0IjoV3CKSb%2FEIaMSlXWaQ5cH4SSjtNwtMfI1%2BIWJ%2F3%2F1txKNv%2BIE4G3DV3Km9V7zKkdSw1utag9txL0e%2Fa8ia9AnhbCvDSfrAbrPzxwstr%2BsbBTMpsvagNLoetyww2gPrH&X-Amz-Signature=ca7c3046af99bfca5aa5a123f007132bc5e8e79e2fc028a1af1f7f77b884538e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

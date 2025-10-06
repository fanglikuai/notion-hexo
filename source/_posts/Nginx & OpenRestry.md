---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VL7DEJDO%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGLwAu%2BrfTEjqEU%2BSJw57hSzX4CSspAXKf9XZb8%2FSY32AiAx990Nm45SU0%2Bzp3uolSbSq%2FhaGfhs65vhEpCmSAgTVCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMaEiCTHUdAnHGT9BnKtwDBYjzC3%2FJLqa4Cxl23ojHzz7U2T3hppVy4NwIcfjtWd4YDi1bqRKo6et6lOddfwksH1UHA9BhILGg3KwJUtzEC%2FQZOPQg9a1J6V5ombACBAsIUl7we13LxbIIGaRrH6uBG3xEqQjUeujA32qBu0rhhPgf6F%2FpdtHKM800%2FRrv6xFrg0E45eC%2BTRiHYF28chnpnibxOz1d6ijycWYdds4plBhAyglm9K8o9ovorItF2jVDcskMfl%2F0eftiYAchOeymLwEI1WkC1CBQmvXkNC29WduKbsQ9egzx9WFzjmnsXYCyevGpgNp3OtXLcGqCS1vcoqdUHLj1vyVUXc6Z%2FsK4wURU5twK%2Fyc0eVpz4GuUHCsd%2FLsPINPVWbZ6pXSfHADR1wNZZmpKD7IOjaou08G8KCMNvufz0yiehon0KL8oSvdvGMuJ2EbiuxlzuwQRGImypMqoowx9WXU1lZteH5jqMI6lY4clOmZy3QLPCTbOxSCK66G%2FEzoWJB34viqhG%2F%2FvQM%2B71WvZRB4eo8J%2FatqyrjSDjRJe97osAlXS1UDHq4M86KKicX07fIoz1Sz3YWVR1fZaAZ99gnbNgLRXqHuEBvSgJvV6Scxlc7y%2FoC4j7yjPu6cIvcllP0h4Q8Uwl7%2BNxwY6pgHuc3uID2YOgyzlfCcASi1APXoSPYMCULH6blpCGShc%2BaV4mOrZpSO0O2vg51%2B9MSJCtM0lczp6l0BXPrDzBFP6iYiUDWBKh4emcKaQZNvoQ%2BRBSZdOr9Sg4SeFVo14Abkld9EbxMCAvA3eaxMnitz9XJMOtDUGQEAEWBv%2F%2FLx551G5wCF2%2BHHswFPLnQMgnm8WkVwQ6D0qPM1D7%2FrK02BPL4oGQ3gU&X-Amz-Signature=1c2131dac290333d1b3faf25ea40b114ec8399d8b68553a9c1e50bef983646eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

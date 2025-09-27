---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCBOF4RF%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJHMEUCIB9we0hhFrNU0k1fWsNWV6YXLuufXT3CMvJY7%2F4h46xjAiEAobqK76bqywEGGgczLngXSy0DyhQjvccgVdPX0sQuCysqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOalcfIHHv0u9LTrKircA1SXUVZglrDatbb6ccZcsdwsiVc6Lp4Ro8Ddq3NkulgqkxnyUNsuccKefR5UyWoHbaDHHXzjpcKrIoad9oCMUeSMB20q2MVIfS47lueM8t3Fv496WbG2Gmq1ki3E6zUiX5wJgRFmlJ4MTDQjHsGuT5PZTMWrgmeT5TTsQP3634bGeCfCFv4mw%2Bp6%2FWDKUNixcqTGKLExLqhLefG02GGWhRqkAiT9sbMCKhWgVz8ZYLHR8hcMobHjRNSNwZbw3N79I6gTlP3NwgIEiJAvkO2MdHkFJg5YZklO35cUIq03QH5APMobBLcxqCIWboHNBwxdVM5TgYWeSWK6ALwD63RH1XsPS%2FrE6BX73e47AAdTKJ0B83iCtvaAqfAq9%2B%2Bbg5fxidl7o5nI%2F63vNGGDy4nHZZ3eA0eAL0gpPxCHpGhocwM6YhxWMheV7LJcFAnow06A7nJJ1DeLNFMZ6%2FVAr66v5Ak94iw0tMFowFzSgv8IPTtAUqxuPrBnYkqtdPkhRy9v9AVqM5VIStZgL7tnRdV66KzsVg%2F9RykYZg%2B9miS9bVSBcsO6ST4%2BaWpYmSCYFJy0lTk46f0y3bq0flPN3YLRX24GJ6nq0NiQXIgYse1GHTUPzns1E4EsylBt2KzGMJvx3MYGOqUB7lStwUczK9t7hZ4zth3kYCWtnnJwUsTnUWTN7Tg1wSsY4p8ujCWIwHyq3LEOP1Riu%2BToXZt3jBZcF9hcg45gG4xyqKhoFaHkXegUvgNIyDLiaatn5vF05J47pF%2FSYuQ5Ykb23ge5GGxXX4Hs45sKtS%2FM9JyDgfgei6pJoCcbLQEkFUpUvirKXoQxNAdPMjzmHzWhIgjX7r3HIezzM5hqBqRIJz6d&X-Amz-Signature=14bd3b79db31b50574f2de7f6dc1813ca8d16e3b31e15118766a0c763fdd3088&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLB5WPXX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDWfzqVUO66VOOeXKaqvl%2FgN%2BpQceilbZyFHRbRG3IeKwIhAN22BomkSBze%2BSVQJGNyQ%2FefCW1Nxhho3V6mAqvoCx6MKv8DCHkQABoMNjM3NDIzMTgzODA1Igy9MVx7jDATHYsfquYq3AM0tdQn2xmgsNieSwHcT8SJyf1uaQCzm1iRmD0vok0mw%2FM6aUVsxKVw1RPpvYzFB8uuAdB4Klett8xvhNyqqihWlkna8dz3DpW60%2B7NvDFubC6fUZIMcCoC6xqjs2bmr6C2JhxPy9nXt3K4JbU6tuy14ufW0mq7Z9whCzNQ1XKdsm6LjkUsqFdW93r6rk3KEurFyuE8V6%2FMDYosZEwoBIGMCScWcxCodfaeBZHGjp4o5g%2BTXUuW9Acq%2BoKSCe7qtChH9hn2HtLOh0NPakFDCt6njQeP1ecdA4TZIvXnYjLJ%2BpCm58GhVfzroWb4ucv7yi9a%2FRVqMWv07YdEnugEO6dTgAQMxjoVDFtsaCZp6IUo2PzK8pKr%2F%2BMXslJLZwOhAkluHkyF1C02pnF5JDOCP%2Fktr1YrnEP4vg8tgwOUskGcRSDy0o8%2B52Mjuz3qR7JNyCiNw7LbmoV1w8jiUstxJkPU0F3f%2FCzrRhhz9FECamWHVTo1lFI4wmen4vmys%2BRZ7OmOpjRjqqqKeqbDp%2Frt2nVfLj7DCpSi1vJfWvcdEuINjVuGSKX1ACAI5KVufOMMNhZcEloSKnKni63dGrlYRCJFcpc%2FhSRnmylp4f00utCqIH0E47WOrqNTHQMB0jCt8vPHBjqkARzTSOa2qQMzPFWL0WQoqAvIVR%2F0vsTo473glfoOopA0h6fIerT5kWfAy%2BjVuS30B2aVJ4QC49i8UAL6uQtNwI6bRwAWpOaXFQkbSV2zsJXzI0p5WnL%2FUgHC23vTPGml1os977mtEb%2FYuFC8CP73LPCy8f%2Bm995YdNAUlji%2Fr1pJIx9pAt%2Fq61ZIci7dJkorTxZsZzRcfPLFmVWpcFw%2BnOPKmcTk&X-Amz-Signature=1d21a4112187934dff9bad8d2f7714cdc3c40d1bea86c20ac2e223ed5a2f37bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

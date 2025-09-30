---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6ARI7DC%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCu5XCb1RKbT7VZ%2FAfOqOg45UwG%2FKmFUkT4Ff%2Ba%2B24EtgIgHe2ksVJsvA%2Fd4hUMs86%2Bwd09UyYMAOQONtRpKXIMcekqiAQI7f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPOP0%2FKlRGDSBgfKKCrcAyOPyceIXMRNMXbT8TayO18K9gSW3iuLHmpFRW46Q%2BtfxQjNxt%2B%2FrCvi6hrQnf1KL2SR6iXaLrQmzO8bTUdFZ7suUOpZy8ObDtx6C87JLNORvWi%2BIBi9Kak9tAG85DFacxcUwCZZ4O%2FXC7CRl6iVt1ZgZpqq%2B1WRIsuXb4eUAbsLRQkeUsomykRDKsr27ZBpwiUJDgn76R0LSAuU8nW2v4HaScUSNApBN2sZoHYpBp7qpxrxFXXkKhQbUoc12pS7BkfIF%2F6OjjqxwQY4EFDAqpEzaWDr0Y2qIlS%2BkKYqGAs9XuoSlQlr0bL0csiHCoRGHfsvgqyNQLpsWcLXL5bfxAN11efOCva6zsNgQ1lZmsBEKW0DACsuJLbyA9JAKb3J7ONhupfosTC8Ar00qKp3zIIkLh7jLxeDABQ%2B%2FF5WOygjBpd614rMQ0q7zaUuYL0QEwZkwyNWNf5Q2NrTDg21PssWafXqGKsvZ3KOhn%2BhjeQg8%2FJSJzGEFUX1oO73Pr9MMZqEL%2Fxzgj0iICaXnihRTb%2BtqtG%2Bvl2xlBd3Pr%2F4wTlwSursIHCveInvu6ndT%2B3SNAbxZ4IO66nqQAeZoUB7sLP%2FJ9ukldFwBI5vQNf%2BrG18RS7wmM%2Bu6cCAk7m7MIOL78YGOqUB%2BFtxqCG6C%2BZ4tYrxmKThdkHXHKtiUCcW%2F%2FEuSxSi1jy%2FMv%2BCJOS9QC8jmmEwi06ok40zU51pLFKCwR%2BnG51U0jcNHPVonaVXvEW3gm04WzcAv8nsiufXqCN44NCdmH60cChQOOQAu%2FV%2FgrJzTxzc83TU6wF6vAzLS0IFn6mG%2FzM6%2FP4vPXFKAFRuU15JK7pQnKv%2BVmorRQc93UePOFhR2IYsVUd7&X-Amz-Signature=d3c0111b1d71c9d64c9402ba5302ceaec30e458dc994277217559bd975329013&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

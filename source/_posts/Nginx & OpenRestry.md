---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZE6PVUJR%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T110040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAcyYsRsIfYpby%2B6op4UMemLFXlSu3L3cFZczxwCO6bZAiBcd7zAyixGoQE5Xb2s4TQZn4c9cR7jwam26yDAls2yDiqIBAiM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMSpXDirPlyyjOlZGEKtwDSomERcEg5075cMKBmUWD6JapVZWaOieXk7OYQW8wpvs65H7xvToz6vvWrLwtcSrcE%2Bme1cAcEhjNfPhkVsgDaSY4hYKJYiHLs06dQzqAvg38y5uUrOXHI%2FjJTjgwn1W%2FlGvTaWnAzOPtzKKKHcAfmReldXvw1F5omB0yTo97fDcNwvze2TeIW%2BPF1xXMf4ArCL0zBxnk9E2744MgeRHIy2%2BPGZhFCO2aIcRZ1ZzuvcEeHl3VtlhyyLchGgNkqPs5qcuvnWwvoZf8WuXQ7QrDUPoStPrfzOk1x5UqPuYryU6xsCVFqhUiMj8Q1l4AAoF%2BdhohKildnhmT4SZZNSAbuRXlTlHjcJUT8sObYBU84FhtxwUCdVFsSu%2FzpKtkvqUppTTT4zqWy5Smxhlr7wAcxQaombnT%2Bh%2BhHaCQ5H49UEx%2FNqO4Wwz6n5HBAoQ6Oy8nPN%2BOMZMbRrfqzxqZ6uYQdg4ZXOMGCT0ZxzLgmXFobHRV7loXVj0UVOi%2FtihaA5Ka5cLzbHC3aSO9ZczZQhoBK1FwFsyeSEuILL%2BGFceWKq4lEPhVFEMGGHgL84QqLhjpH5NsCtwGP6p5pS12m7cH3X%2BI4LqPnMuMg2KB3wQJgzMx5vGyjdOs%2BiCg8kMwgs6syAY6pgEKRgT%2BVA5Fz%2BhmVnxZQXXgi9g%2BByCro%2BQfak%2F%2Fx%2Bsu5MDTeqkEcWJRQxcj3utX4a8CfqwHtmaYP2HgnDYcDmQ2jGL31mILrUsC0A1JHKnIaWF4XA%2BOglc0xaHZMm43W3C8gosvjpIQKia6fKlKtMSZt5SemdabMSEpoOZkiCel%2BKHlusgulgG1jRq%2F6SrPCPJYQeRi6kVXqg451HWxb2j8pfW3Q5S8&X-Amz-Signature=cef85874a7fd714897bf65e5f19a617dcee985f479c19930ccf904ef48147c33&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

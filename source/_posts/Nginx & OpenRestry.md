---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RZ3MFY27%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHXHNYfnlEUCoTYve9O2fRmLyZzJFOq8JydxrtA1vvYpAiA3QLf8AC9SiMrNmg%2FRQmRad7HSZb5eNq1K4xfMvpKj%2Fyr%2FAwhtEAAaDDYzNzQyMzE4MzgwNSIMyOQzT61Z8Hz7XHpHKtwDqru6ksWW6%2BHB8TK0Z8TIPpu%2F9BSh%2BNteD9nYpvxQP%2BBrBrXpBSsMY1qWXl3IYzskTVc2U829tJ%2FDkmLZ7pp4DXxxNwBrjHXuKXQ%2F%2Bv9%2BGBEp4WSeYOBZhgR63y2Yt8NPBtEf0waDPBcyEH1PMm4Kb4%2BTk%2FU%2FjvNRNmn6LZy5M2LGlYDGuvHojq3VcIwjMbrq%2Bk5B9BRRDn%2FWuvcb0wenNlUjPoZS30qYlCXRUEkU8ZIR2nWHBJGXNjSbqhvZ9fKKHTuUc6F5ejKUnL76c6I0Rgqi%2Bv3EB8E11UfYquMhuCiZaARn9YzXwAAJdLKKZEEV2wd%2Fy5iRvTRZ0J6N5HXbTfonzkIwlLFb9Bp8ngkpomnYvegz9BsxxgNa%2Btx2YLTxH7jp4EgHXqMLbPGm1nUkx9dEicQ7PUk6G%2FRr9mW3A4wU7V5c5UZvW7AFF%2FRknwXB0qDLKXBEoErxL8NpglEFNfhRBnRReHenagVMQthjY7Wvo7589Vt4y8gaPMu%2FlkFJx2d0doRu5ynSiApjXnmiOit4roSNdeD%2Ft%2FoRheuwTkTGEK32di4ri0zXeN%2BTCt9kDMmjKvl%2F%2F2kn2K7Vrx7LO1UBXsdkNtFAPRg828WQfi5SyUWx2K5ohtPM%2FFcwwLWWyQY6pgFt1JeB8KDLwEWOvcrkJvtfIfIjB4JJkzDnooS%2BF8%2BMNZWe7QpMjO696HYt3960VLfgj5RJXbwYlWPls867osAjbykO0uhf8OKonMYcMqbuCmpqJAUFM4s0pasq4lIMmyyfrGhF57lzXmpg0IZhvSZQ0sHSNKegRPNuskAnL5iHQZyB%2BOJIePitAJmL00babwfCq1GuXyjICv4zD2iZTdJNJjpT%2F%2Bqw&X-Amz-Signature=3a15af46531e3b8c17a7e63a79886dcfef0f62f30bb89da7432a7868d10a9db7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

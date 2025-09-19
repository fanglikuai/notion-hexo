---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDBEHZGG%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T110053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQC9iyWSh5C212lYj0PUaJSyjbLdCcRepVyar0Lfj1g2nQIhAJFth0EKRce6fMsZ32omj3ndLATcNTolrm%2FwkuLgCkD5KogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwv4WDwhYkUUWb%2BSCkq3AN%2B%2Fg4feq%2Bp3xp35TeEbEXa0LqY0MGU00WrdEppFSUNAAJuT%2BSCJ6fj2dNG9HGyGTkfX8nHEssAOmfqDvPg5oJxQt0Q4yXNgZ3iYKYwc4rl9zPzjvsHAw%2B5x8j2EnBYtyHQWUhC3C0Q3phz6slxltAodgU7OW%2Bd2F2xFjZT%2BKeNyqkDE2EXE4t1CALje7ySP0oCT6muDb5F%2BJKIn3GzHitbPnwlveVKBB7Ot2O7dJL4vdgp3zZzY%2B%2FCPK9ztZ0axqNIaT5WU%2F%2BxxIg%2FRKmxuH2cMxyd8fWcuzB2vp8d53a7VID%2FBxuI4ZqznlLUd6WtiC%2BVIr%2FLr491e%2F3rSOs0bFEECdrEjn%2F%2FCBMD%2Bku0VmSrkAh7gTNrlmZkvv166CFgAV5Miz5pFuNAW6AxRljUesBhkezdJSt60hoJoxXdbhBI65QgPt74YB0LDInL2oV2gFFBveBkapHqJ639jcXoAqDg12Un%2BXdg0Kvs93hR%2FkZvdY6xiXqh1jlBtEbzxOArFc%2FNMifcmUaWFFKQ6FEHTOvcuvepyyZ5HYSQQPY5rtYWrTUEnEse5v6HJ6YXmgLwKJgdrx0x2wOdBiHypTITjXPoOXWJf2Cwk7IU%2BLGM9D61hAPKgfnp7QfE12CmqjCX0rTGBjqkAYQsYdR0SsH8ksnDJt70mHYCQnT9H8bLTc8gR8pVF9Nrm5uJ2pn5Hx59n5aLnwXWWo%2FgJQe2QWOXegDUt0iCQd8S4kaUbfsntcS2O2hwBfifalIGmfGRHUMG8hcHXGhmPpsqheL%2F5vWnL%2FSowg1C7Pksbvx5xuacUdE7qjv6Ik27P6Tg7%2FCBXaLAKBZG0g9LKLKcAi6ha9CMFPCfw%2BFfiRZvb9Z2&X-Amz-Signature=8961f7a2abea03dba22f86b55c67ea31220b8215c75b22f74ac0df3918a313bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664E3IEFPJ%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQDhx5OgS7ESESxOWaQvIjQDZSzA8htN8x6o%2F0VkHANrKgIhAMRg%2BU0rRPxzKd7qhL5d6k6nHk4Z5%2FAPg7UtF3olNtBYKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwyBBMmHTW7Tg5AVjkq3AOuYwd6CWsTRATdxZKTBxztZT9Osnk1fPsAidHjQW6e0itUrVf94P7mgSbnxeGfA5nr4D%2Fy175JqmUvEQ%2F4QwEogPEJnKIm4HpUt7BQPh5weRqGcJvj2o2TQuPvvz03cCsYH5dH8gL%2FujOjZCyOC6ns3ELl7cEpUMerIuQeHRyi8LT6gXL2sAc7zcHwI92UzIaOEHbwaNoAxHgud8x5QWn1anzgVJz7czL6T9jmZltFFjBOoJ3MFR%2F%2FwkZHMTSLR4hWrmULLJnQpqMHiyVsBRcrdzpyqJj%2Ba3xqOtEeciEyE6%2FBTsFszGr53zMPKhoWWcR84gYf4HRe7SBWZcCn3BMFRwgiKV8dGrm2cww3dNEi9LgTfCU2XQRGX%2FtPu7xoWF7tJTQ%2FOn23ZguzGDJrXgopxmfg5gatLIfKNPgaXVAJxKW1MD3D5RK9pfOgfdV6qbELN8zgrAOzayhIJ7nNbHdQ%2FZZ345AytFDgMZgvR2bvoCFcsiOZUCk66a1pz9RrF%2FQX8uKsitM3OWR4grCtJ47ThqaWsDzhj%2Bl%2Bg1X7VELdFR1yABloq2WNM133UBlqWIx%2BiuwQjyxXkPy3ammXS9sZIiTZWmvigwI%2FKewT8oGgGBAWX%2BbUH8JKbddH6TDi5MzHBjqkAYqedhMlYKOPTmJ1FD6dWjcwU0lPI8ASE%2BonpBr1PL1lu%2BBSTqmXveCtc250HypJF51WuP3Cqm2C8PA%2BCkDn%2FbOQN%2F4I%2F2I3VAyiXc%2B46YRMrnKFpgM3zlLEVcJFskqMwcmmyV2Vrabfgn3qANTOoK7b%2BQf3NhXUiVg8iMV9u%2BHxyQyfDWxFfl1eTGwdDr6zN8%2By1uvQhgfvjc%2FwilHeQH3mW3cv&X-Amz-Signature=db74272b2776ccac8e810757f4c7754268095e033cf855b09ecc4bc360424f87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

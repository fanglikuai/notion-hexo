---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RHLNGGDP%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T060135Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCLD5VODVh2TWu1hCW4NYUSoft6SGJ4P9qXf2fTLU75ogIgNyLeYTYS318zEuARaydqJPbWGjrv4XNjYwcNqjNvdCIqiAQIvv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCwiwGsOQgakXBCJFircAzhodBJsK82E6rISN2yZPcENJHaDfjxZ1e6URsKGx%2BHaGmIAdhTet0CuQznJO3kzXS7WmMBhl6UgORa91JL1R6pTEekPktz%2B47%2Fh6hQ7gpkpioEb9Zm%2BBt1hgaORWN9%2Bq7S4QNJBGSlGB%2FnzGVzWXZOl7dZjvbki0nhafRcd7WCH%2FbJfJKQ5i1BCjqMAMFNyoRzBde8PlRtvn4k7moKUbIcH7I8lV6H7PY2SMqz6gC4HfUt4am%2FbBcmfd1yUk2QTAdBnkZYIEJjk2b5gSWV58CWS5Di5j49TIxTALvdB0VWvlrXPJz14h9BZnqJV8Eo4cT9za2cxjWnuwVIr0wAd%2FEcoqgTeAaZHjkAJqkQU8zik2WRXxQxheCGil8Cbmn01WLyqVDUebtAuBv%2BCxo8gxR%2B7ozePRDuydYVrvJKiunH6B1ebmdU0Kqo3JqZ35nUi246zZdHtYI%2F6lmNDC2fG2QrwHUU%2Bv8HPF3XDXo6ksVvlLtkFK9FlGnlVmn1F9cA9DThBaoggQdeGid4tA1KjEtbRICbdE4pmj2fKS1uN3I%2FYU7RnsHSHlUBx8i%2Ff0AGNz914cp06OzXAe3qLJ61mfrtI%2BaHXHl4Wjcrdd0PdZPpoW7Ex6B%2FyrfLZ%2BNG2MIL878gGOqUBLudDmQNhUhZIENVkpasY0w4wLXK0BhYDYl%2Bj6PEd45qhtJyoLMIGfZMehETBCzc%2FDWCM%2FxSo2LTH1se8tXjtGISJ1%2BZd8tolcB80iz5hioPMVKQZP3ncBUFOJKpdLy5hE9RmtjF%2BYKGLT%2BKzEk%2Fu0IMHZ%2FDDHYYFmeroItU29WpbRXJJ1KDhxZpY9B%2BJoPmitBb2iP3osYNjgr%2FU33tTmmuOCP0A&X-Amz-Signature=841f107697faee99bc7c2f06e2e39938b09e3ddd8f7696d217362f5cd6f60fa3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

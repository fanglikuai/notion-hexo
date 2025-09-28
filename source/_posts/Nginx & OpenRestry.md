---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SI6KBR2N%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECoaCXVzLXdlc3QtMiJHMEUCIGmgbueBVh3QgH%2BpU4tN70BDdlnCxpJ3JQNekWh0j7YuAiEAio8SZrVxJUrIyWT9LTmfOEsfIVZrI0taFZCRfTIimkUqiAQIsv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPrUIgtpxABY0gfZkSrcA6402w1BClLfo0NTobTBWrlSSdvsMk2lISUwZB8K69llalWvypmTZPfQotJGAto%2FlO%2FqeJqUd8BVFlNGES23umVjjyj4RBC1mTJJEJc7JojG2AHKxdn7IlCvdReQhvQKDQV%2FAud9VlpDH8wH574VfZ5gr2dCClRVSjB1mpPkgTeNkR5oWAR32vkS6Q4ycodVxywhDMINc%2FVgZMKsTedPSXfJjC41bfxlrxy5HLZKhtoLJoTbZ1N6jsoT16dDQ3zRob5D1zYH9nAESRBzwTDrM%2BmMqBbaHon%2FPX7NwSEaO7xbZgY5k%2Buhd0pV9FVlgBOeRYPVrjvt0XkhdcTKDSSWgMbpjgZiP%2BV06uAZqYoj7Z3CNapUfSa193G6oZiPlr6Jfyh4avFeS9kLdfx3%2B2%2B%2FN4jkORl2VC5I37CNA0gVmjQZlXOm%2FJKNFGJyDfKJCb8cLMbo%2BqsTVJ1%2BpWNCJYTi1zaAW5GuDEWgrFN62xNnHVCDia0Xwn5CsGH46aGKy6QN04kLHiXXNyLpCEQH7XVIl8a0c6E560%2FTz1lGrfImombBIN6GseaQD8cqU1%2Brd6eSYvNEyMkEzA%2BW6maJQj8Bpw3g2OUVHuXrqOtdSUCo9DS7%2BkQWqcp%2BENZnDtahMNea4sYGOqUBOQ5Os%2BuSMOpMd9%2Ff5%2BsDKD9ogB0R3I0mkDP0He5oxyikzG0e39zGFVCWvgFQNDH3ZwuRlIWPGnuX4tCX9e%2Bk98aOjwgCQYtJaaCF%2BjRXxT%2Bv54RTBgOF8MH%2F4n%2FY4tUXNe7GGvoSGn%2Bfl05%2Bx3HJgciNUOL6sAanfX7SJ7MvpkE8OufkPGPqHeue%2B86qPUIJ5ykSJJFzKN2KmA2iWbzneG6SztQs&X-Amz-Signature=44566937ae545ab2e3f20b3f8b728bd4ce2497b9dd332a2accb18caf23739407&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

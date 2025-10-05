---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667EKL3THB%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T210115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDE%2BhBhlEyt8GEvijMxuaLp0RAUeUXTZlA8BkDkZVgLzAiAVJ3VzfRSUTehd9oSkh7LVoFWrcz8azHpOiL2j%2BHkMdCr%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMO5WzsoDSkdRfJlGOKtwDxWXNimbqjxNmu8hrJbVi6xoAGLfWhj%2BuxIDwGGNVCxWrt5kg9QuowXhf%2FcXuDRyZUe2O2Xa6tU6kAikGXSSw5fvDv7PEE6%2Brpd7Dnh20KUpfovk5yjcSJGaLJ08Pxsk2kouXz1SudyPq4YQ0Q0Fr7FQ0y55HzjtmpSAYF67DkXZG1xlyNz0ArIGJ%2Fqwlw2%2B311vF1bzZwysjuuA3SPvddCgSyOQQsOGdYPPWGcBmb%2FaGKutu6SfZDivGj2ORalZtrOSvjterLYG%2FUkS9C8VZuxGLsw3QdjXXZe22Ue01WwKj9yR%2F21NTG%2F1Keam1NSOYiDQBAaNYjQ92m20RHB2kzNdX5cT2LMR7qs%2FX9JdtSip6OLMAmerEQd%2B2Elf0HlBteP0YzDAnmicfxpr3Td4Jzow2791niW4itniXb4VeIz3%2Bu5jatGi%2BodbN5uNEB76VReNBLRlUR6HJR6kv8bun9%2BkDQPfgrcBnpOkgEH6ruYe3trSEeUxwWjMPTe0hHCClSj3q12lI5Wyd7tsTOIFKpt2wW%2Bxj49BSrUsEJROxnQIrN22JTRAgp%2FH%2BP6snMHsvcMjsXDdanLkLJXyzqiCvVBmiHIbACP4Cx55Vaway8b6WeqIPsAU4rbuitXQwzOiKxwY6pgHmne1EJkJoS9hexm%2BOTU1%2BTeQNLCg1CrROyD9XbmwhvKpMUVDGt1QjkhwL%2BFaCwAEOGNuNKzwXRXSrjFzZ5ZLOZfP963PmH3n2yFM3Ohv4AR%2BsNCIwAU0KhkHjiOomB6T2Wg4cjJ3jUmTlBffLGZChNOVNj3UneAwJNACu%2FW9gSliaI%2F191iNlaAGZFU9ilQ8k4SvIxi6pj0U%2FDLkKY01zNm1xHR0%2F&X-Amz-Signature=fd76724525a1aa3a49bdb7470bb1a45da7a35f5d3ab9dd510a04b2ac5320bfb1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

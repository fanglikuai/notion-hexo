---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W3RNPSHC%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQDPG%2F5Sna1A1%2BVte2LhNBVrlRWX4Olx9TqPVs5E%2FMP0IwIgSVURIy0FOBSjfDnPuLOE7QcykHkbFWI6kB7nA7U9k6sq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDI2xrquAzNcMoUrw3ircAxOx09oqUGU5GnuKoc%2BmMkUnZ0J%2FZTsJXujL2uZr3tlhQzMRsGXiFsfX2xXPDwrveC2uPHwaM6qRo4zhBj7H4%2BjGiyDdtxHoMFXAYcorjjt%2FIeVtbHP8XgMF0fpZ%2BxuMN%2B2cVAffnOD7QIcpdK1yGr1PljU8F5J0K2QSdlO73U26FhYnwwImlJx4FC9jgOs7FNz6yKL8HCtekJ1WhEzCuEykoLVONYmfz4%2F%2F62a5GyIVXFzsBweT7NGP%2BMTAeqPBT64CY0N2ily4blHRgfSA6z0zJcUbdOrMiZ8EAgMkZ8YbB6JDvxU1EKDvvPxCiIbjP9D9C4d2m2qThsU4kz3zeyXhexoutUWU5ecB3RWOmZd3PsdL%2FjrUw1HK9YcuOPoMCcl0l9dqGXriYLVibwqI%2FYcu0I2z3OYX9%2BLj2J3Pva2kveY6yAb4RHBo%2BnaVjCVtlQKFRoNfXF3WWcW5LRQd%2Bcqpm%2FiODH3Ff%2F7TeIRahQHw0f7ZfKkJqY6XCnNekFCMsY5hZTbRh%2BwC4KduBX537S6pUeAqiBj0Yn%2B5rrbbEQFyL6U5TDYxyOgrwI51ovdNIYF%2B0Hzavssm10Wb%2BQMI24obOnRZZWGHOBXnC5mxz%2BqUBXmB%2BDUJI3PID22ZMIDhlMgGOqUBmiTVuVApx3vY%2BQifWuZy3yVDoIaOeWHYAcoUo8rsTVSJS2qhRibbH0pSo687jG%2Bo8KSuQgRwzjflLpz7aA8vP26haeHCzcuOIgzlY0%2FUUK%2FkGdNGINRAJ%2FA4LHzuRNlCzrfVj5D8KQ4MJb0Qw0oPGAUv2c8M4IXmhBdwkfzpxoOpXdPe7nDmlcRkY2rpHrgwMSqr07yXmCuHFVpAcJ4ieu%2B84ZrP&X-Amz-Signature=19bc0a74dda9d65c60852ef69ac74f176310f7ab34bcaa57af826d7554c8fa1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QW62VZDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T030050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJGMEQCIA9OJR88xrsHQ7%2FEPkqv5NigpoeJJ5yu0ikG7rJGAr2SAiBDPBmFVi0HyS6fJBtyM5VuOJiVkQMs7rlujnaRD4tspCqIBAj8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJXqtklNOMCiLioLBKtwDkfx7BTMvJdHZZTYngSdMMfq7C65uB5IRtalbTsZpdCEoauySXbv8chj1ZaW8ckLJotf8qWfonP9Qk9Io%2Ft%2Ba%2BVcV1bdJDo9HjeXDmXwN0odIjR4mir1DWOAXn9D0JR%2B5d5MQ%2BTPfgVtuCLkl%2BjtTmfev7ZF1xjjeOrDQvs1OsONNpoHtoA%2Fbh42hj80uKiPbSRHY%2FPx%2BVN%2FFJJCcQzuEoNItNXK2W%2FyT8iAL89PgVA4dRotMOu%2FVmWLIOwqzRFZ09ig1K95T1XzmvVbY4LatKiVWGvCju%2BcZpiGnj6VTaf8fqCtRJdDZGoypAMlZd4O6A7mMEbVDObyklK5szPcCTdqJqjdiR7gxYGAo7KROoldYz3Kp9NxN2bAN5ssW3vl9op%2B27KR5yWbApUzw%2F6kQbyoGptskpxN%2BEIY%2BBGa%2FqNmqdD55Ejfrqb5OntNb0iKA2cTfy2b4Pbfik8qHCdYJIm%2FspBzmgHSvLfuE4jlKgVyb3Yeqft9VCAwIb9veQ760STe1N9DBZdFpXuSnq9m%2Bs2An5pjdp2bl5L0%2F2S5zLoW5XlmrlXx3Jj96bALRsBydh8xSdVDyKNvL%2F%2BcguZinmC%2BVuECHgQrP9%2BShifcpJOm04v4DTLrI8X4Z4Qgw94SnxwY6pgFKPMtoiISfZunNhTAIYFzci23W8puUsbkL1hxiaUdR%2BYPA4bxH2qhpnwPWKpKY%2BIUImhxj48q%2BhFcnjL8aEIz0ZoiOV9FvyDU5mT7BPS%2F4D89mGfm4LpFdNGMRrSB4sIcUOTiucq3GA99VV%2FY%2FJbGfzLmKewxFkqnklLg475QVUvph7JVK44JFKrO2vvOlRDsBD5V1kYYfissfwerY8Bqvl2DQ2f3d&X-Amz-Signature=831f003ce12a0f298578fe4fd6c607ca32a92ef13d47ba0fccce2c9c73745c85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

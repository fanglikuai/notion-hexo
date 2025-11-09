---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665YHNERIE%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIAGhXTWR2z2yfUc1XUocfxFpjnJMTmEjCPr0w4R50XpNAiEAtbSbElgpuvi1BC2Lu9dkXRTZ9%2FWRjsVjnW%2BfN%2BwAMjwqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCPmGMVlAmB9Ex5k7ircA6XWoj8pHj2rcikBBlZ%2Bw4f8xFtXlRj27guNHEg873BUj0UoxQhZSLF%2FM59B68dB9SKqMVDgpNpR3s5RQtj0X0%2FuGCQH%2B6WJzX%2FyNZwo2sMjn%2BGaBTa%2FABRqjwhTJBxQr3UAioGc4HvnZ1TTxLrGYxYS7YDPinzHirp4sgKlnFzMv97WNcCY63CfA6QZRERSkxyQIlrrUhbjHRHXhQ%2FZlfGs72qVrZORAPUSfnOLjDOyqrtGfiQyIol9lAs1WOVugQxV1P9zCctMq8Lqs34XToUbhMGpB5UrPMaz3zl4IyCxqEG64MkM0fgEQdy3KDx9v5zu34Pz8OgAYdfr6Q3%2B3Lz5EBHGiiWVnGwHtAKxS1QJUfUGdX4ieWxFNhNRhULVrAyzpkQ5VYG5SIoWAJDSdCiriNXHBWgJsvRnQNXLf9eZWhBcGoLCi8WuXdFOiQjIPhSWJQiEjKtBWfbcp65hRPV4caTKAlBHfynM5E77ARmUXeJGxkgvF8sZL6vmwzeOv9PATsG24NDFaFHF43x397J814FPybvZx%2Bb9%2FYAVrO39b5GWcfqP3dWhEG9GXD79zKoUdTEjnyLnpYd47GmmpMqk1uYjqMgixXJk3eARwAP9ofLkTPCycVR4%2BLWSMK2nv8gGOqUB%2Fz0k5rx2OxmjKVO9SKFa0s1fcqI4HalflS6VRNewOViuCoOpfUxo2%2FPTxWGbWWiBBbW3%2F0nFCDJoxXHUp6l%2BpEfpSU19SNnCzesyZ3GctjSZ8qLcaZfaZ%2FY%2BBe31jB1jcIeHfUHXcyk6UgvKHFHmD2rmbxAnK8r%2F2rgy6OZ0Z5nmFE15kS2YndwhOXCkxHwBxkj%2FdGJ5xUuVC4MrwDuenyPdM%2BCb&X-Amz-Signature=bb432fa5b9a07e2202f29d9adfd3ea117baada489b6f173dab6478b180ca4c3d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

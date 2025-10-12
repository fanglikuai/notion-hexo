---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46663R7PB55%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIDks87EZcuweVxV7xvtsd%2F8UII%2Brj%2BPecLZcS%2FYel2q1AiEAjtCLBqPWDKnQ5VDMG44t594VoiMVWOh401w0RertUQQq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDBizeTBRO%2BLXlkX05yrcA7T7izi%2FRL0lg5EEC0PYvzZMCcPzTVbdNRO3JuFmRbCeXuiK1X0M5m46upeLMT9mc9cgXkRF78sRWGdAI%2FpB1hQ4yma%2B9N%2FLElTE6XqdUkeBznAUJyrx9HYCYyuBIZrjXl%2Bx51LwDuni5Pi9Jj5%2Fz42ACI9whnjaTlsfZz0Q3XZ%2FUQTEqOApRaae9fhz%2FDUm5Ji5fGTYSvFMEHTQHUhZ5gxcfwkoC7Qsd2KnztvKWXzB4gZc3zwVZDPz67ROnUhXEkSHGV%2Bok42wdC2OsLBkArCygSf6eKKWOiLV09IvGK0aGFu6dU%2Bstt85HfRbpND%2ByY9MTml3%2F4%2B%2BaXXEBziuvvDO76CeqFaYBEqj%2BlBBsqUbyvkS%2BmoweBIsDO3naLx%2FofsJR7geWuCdNIODfUtAwX1vSbyQlhHSFQEjBYjb5RHRDEkXXLnsD5SeMRGhLdwj2MqCJgrzUSzmRz%2B1hgvAzt3WvnK9V0ILr7GUPOiM41S%2F0XJvQ9QyfiMPp80L2S3562qhhiX%2BX9p3eJ5AM692eLBqneWt4GBRMJF%2Bej9kcBCVVa9ogo7l2YHUS2uLk7Np5hEILj0PjQkLv%2FLy0tbRFn6NhAbAWkXxlRhRicYSe2Z%2B3ZglFtsgylVFkpQ3MN2mq8cGOqUBwyZIXH%2FRDQCVG%2B4DsfF3E%2BWvWtwzBmEX8gclL9slLzAro0cdmzBSmnJkiqNqe9u4QRCT8YJuFVB65%2FZd0F8Id0tddiw3M4CacI89woYpiVrgr9G0CrP6e6F7kl2Rkf%2Bs2WNyIPaaP4ej1Wp5aIeZ5Ak3S8i6iqBhpEtMWzSKSg1jtToXdY8fYA7PO7LWJ%2FC5rprH0M1fzLxJUIYMpNNdsdaTmFUG&X-Amz-Signature=1599a2bfc78c7fcd1c0526d9240eab8dc632b33e539cf5a690366073530cb43d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

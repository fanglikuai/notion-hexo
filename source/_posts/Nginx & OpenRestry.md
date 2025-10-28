---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662C4EVL6T%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGtRgF%2FtYN2%2BLAN2JwDPT9xal1yjIxLs5Vqw50gDcCAAIgSwqXagQLzvs3xPBL%2BHhWPl0N5ZObrlSFmjrxeoye2EAqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNRTty%2FrJj3s9%2B2CCSrcA8HgIToeojlE8ZBS9kA%2FBggsEQ8Vq3D1NzzCfpLhH0mQoe3bJEeaUpqRbhMQDDQ6vk2COYCt3iLxr5PYYM4FchvAVcaLMEP12ZnoO%2FGPsJukcd6Fa5Ld1ItkF7%2F2YTk5bkNmEXpat0fs6gfKEfwDAwaJ%2BE3tkK8nwSe%2FTsCN%2BFomS36sfHuXaNYRh8blBDH6yPv6Lk%2FHbfixSwHcPxpdJPBUysnggfwj%2BoXyViShi9e1sfiwgiE7Z5ZUyFw8OpDxmAEQvQosxo5nTyRSljAq4SV%2BMSwpnjUt8%2BSREpI%2FN0y3D6c1vIirwLemBfUzWHFyVU9VI%2BgRNpGSaTBmN5hgqJLI6ALUJ2RLJ2A5XHSxBSWNIgB965KdPJc5MIw%2FAwB9RvLu%2BtDqeKKntJZNV8viEroFf6ifUuSqesPcBQS8w56OJOdLoYADhhUuxtLgUQYGciIv1zilWuqO7%2BDtvbnp6VW%2BvsPy04zgFQ2eX2dDeylVVZoSay9GvMk6Oj3Jl95%2BJ9m59RmRO6%2FrhRqUTrE8bNqTchaO0TZdUTYVkPAQ673WqLLzs%2B1jYVsWtt40PnCZWEPU%2FORNr5%2BcD%2FhZkzzIJdzdi%2B%2F4OqX5d6rn2h56dwgcjWOp5wdlH9a%2BG%2BVoMKK7gcgGOqUBH9R3Jv6gzKXChmBM2URU4htEs9NabdNts3OZdWZ2m2qpn6vkyoghXuGRzPYG%2FQB8hFbZZRPWPaqutLf1C9Pga6%2BkgdRZLxnQUchqZaLX49fnt43P3zAT3UsK3Ma7FuIfmhqt07jqfe6WZv4nlzEriE1A5HavBeHxoQ%2FbMOdS%2BKa358LVlwAv7qMKZVDRPOQL%2F2yqdxUEJmQ2dRMx%2Fh8Cqzs7WjH%2B&X-Amz-Signature=efcecafdb71cd0f5a7851bc03a8c81a119d166539e757aa9a346493f8a69ff2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

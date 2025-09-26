---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RE6BVMXV%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T080150Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQCPACzokJfpANSscZoTfEhDXHVCyEepbWv6wKeVVX%2FwLgIgRX%2Fdh50y65yNpvdqViH9c98lx3tXpOcUwLAISTkqZFMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMKdavrYSUZRh8sXNircAwOtT3z4rrgHYY35sfU9b7Z4a9Ej7REZGuDRJbnwYyUOWtyiKsYY6XPFM5nk4kK8YwFq4Q9le7mdtJ1Qc3x0RV5Pvfk1%2Fja8N8JMuEIAEuxv8Sv8VypP8bPkKLellBlL3r9DOwy3pZwlIum5VZqV9O2zOOKysdFt1WQ5kcBS5ZRu0dUOujjKX%2BQCGr%2BiQyMqloigJNDFz824IqFdERHCqvCUg7a9GieINOGU6DagFbTSAr8U%2FhKk4CHQaIbv8hMTUvNbHNgoM5URf6glzi4QUBU%2Bzz1hSIDOHHqigTluNIgqjZva9MjR3V8%2Fn1RSQKOY7ZHfnetGd9jdG5EOQOVStqlLfGrKZbjPyetyGnadi611fWLxoEb1FXx4PMNDUCvCEAv0xRjoshES%2FN7LWMmUo6%2FB%2Bc%2BrbrkVHejQTlfVdnl%2B%2BWcKEZsNdZXxQPLA%2FVKW8V04brvOB1lYEGS1SsiR4gelldHFEx8LoWqWMhFzWAVYlnvTNVCJ41Wlv688OLS2aJr4%2B6INqP45liSwWqwzMSnefzSAdHA10hHtMX1WMUE%2FB4l%2BypgyoT3jBN08ImWe1Jpi4rtA5lIo5S5NA0wIZWTo88oSMH%2FGby1f99wYUfGGC5zT0jY9KXemIBfCMOH%2B2MYGOqUBWVPplpV3fKeFv28H%2FFy7FxFeCU6EiREAMrmyoKSm4E46rW2jYZmX0pUo%2BujhYV2i90JVg6gf%2F7IFUm5UOX4tvxmD%2BqcNbEujdOas6m4sjNUwNl8ccN%2Br0spNC9h8ZB8PUZ8AoXwGVUssY1mlhmBs3%2B9CEEn7g0pGJs4aFBdWJZTAZFHyQo%2B1UWOnS39wrwjPRKWP0h0xqV01ACcVM08tbBHk5mbg&X-Amz-Signature=23f816a095df0f41288b621c8b4bfee48a474a3ea0216b8d8e25c6c0eb61fb03&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGZWINKJ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T170043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFyLwsgm4GfkEyfsJ8UYpmp1EL8KaqgrzwD82znUIsVvAiEAxL92iFaCe6%2FXVJ7mIWgaq4VPgvPeJJHXoGUye6SkEjQq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDJyW9wJtht50pzWMBSrcA1G9FRG7dMPcROEFl02NU09tNuWa5rj7Bwkf5Ke5QRAiY83Z7gccg4XJkF0aFefA341CH0kHzxv%2FKgK8F%2BJ%2BrdqC%2BGnlIphd2uLxAgxcVj4kAO9ll1ZBR3aiC3ipTsamAbHo7yHEt4wWM0tQS7QwdlBif98RNYHGMzGQDy6aKWCqqbFpAo2EOZOrDl2A%2F5luc1uwuxII3LJMFj6PF8DZt4AKwX3sInUQMy2vXeqIIsoy9jhuV07f5nMyN3jpsVWa3L%2FkR7vQCwVdhf1fQkTbWIaXvwk8toF0DFhXlCT%2BFdtQTnSDF7mrZ%2BD0KQt2n6qnXWmbbkYNKc3S8rPEjGcqYHUDOEFo9S2AaojbepEcPag8AfcjamCxkXiT8mIDSVCyX1Hwq56atDvCAituDfs%2BKPuh5hHA8qezsq8ZMmZzBtX0HpyxgaPK0EvM1P4cBbEVHo%2Fmybal3Av25DCnYFhx3YVZUN6GKo8DofIyUWqj0cy7wD4ckuPWnY%2BccmIMFl6uf6nmdr2%2FRmMkKcEuzSjXACEmaIUh6Z27V3VAox%2FidMYpWX6yL8XMR%2FVEeR%2Fp8D3NEZ1gL0xzacqGtzecv%2FUfI3%2B6vLoFZdKkNT8bm8Ht96wmjDZRnQPJ6%2FjUFI9GMPn1%2F8YGOqUBnMrKEQF%2B%2F83H89WFliHgjDX%2B105xSWgeOblHTZear8B3x%2BcRXd8A%2BOFad9AaoclhKG5nEvKx4svcYTfcxgdzLoDkK6qkvRzyqhpb%2F1oKqXlEUMwgkZmRbnZ8wPpsQnugVSLLhcSX7vXBN%2F%2FTZqQosEqfbIvuBxv1Uvv2d1KUZVxWXZSIsZkEJkSQtjnHwm8IYS3QEcgpJtSxcxwrM%2BdH9jONjOni&X-Amz-Signature=feeda26608942f6c4f5609777c676ec076fa4d51ad269d25ddcad3da14e6efb5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRZLCGJN%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQCCYkNXttw5RWHJ3OBr4TbpGxD8GO5PGsNmswbIdgrQpgIgRmx%2Fr7HMZpzZgGCYZdKPdL0UpwXSopiMCW0gQVIj7xoqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKrKDgCAVMzNGI5DVCrcAwcANL9myXWY2e%2BU7h18SQCrmGVfdgbDFq6LVFE9EINnEb%2B2RT0AAKfrrsPW6X6YkCbqp%2F4MOR9dpoO2%2BTBL%2FmodeK%2BRegMnPu6UWY%2FExAKo0p7HLbVZGUnMNeJ5WrUvrQ1WzmPj4%2F%2BLhpy9yRxAS01S0oNOqou%2B3WAFon3KD4LE6Dzu%2FuupP1ZivbXobFdOgVzAja3YCg1%2F25WO5GmUyEqHE9n67tSaMrXckVNBID91Gyo31mhyDGZi2DPV%2FzCNut2SAoFU4PM07GdbW%2Bo0mh5lhb3YAysMPzcgx%2BJ6ofR9R7lcLwCymMn89Wx3LO26nMDWk5D4EwnV%2BElz5RDjBZe%2FuFGbwNPrGF6r7m22l48gMydyaird2RvGYlk3yPxhbF7SXrYlrC48LuPRH8fmxAnJSHW9KSxAY34ewe0T4WIRVbBWdjWZnnV7jsLmjZFHQahtIVZpcKbiMdEq6uGou%2FF29UXIKwND4KrTBlzloxLyeqYaOjdX%2Fg0VLY6FpWsCcKTTuJXRaD1%2Fq4vXlC7l%2BUyJgIycyfRQrJHmrVVbWs7N1A1dSs2m9MFCoBwT6ZWwHsegjVgsC9D5izDGCCJuouAVuq2bE6vOdxo8fR4ujFYoRQgPPD2J%2FUIFOxd1MO3E7cYGOqUB3rykU%2Br8k%2FkXDRGWj1DRxoHGYeRRD%2FohfR%2FArdbwKsrgi%2BA8ToyP3o5H17xmBMZye9cpynvX3vE%2FEMcTWs5fcPOgtZmgtS2dttYA%2BqCZdNG8dswcjDpT3HlizvCEaxUV42NgwdWQCGWB%2Ff0RPOxmcuCz3ZFCtv7cS15ft%2FmiTVnVEy6IngP6bL9IrE37aFbaMv1GhrZG2lvEQB0TNaPSqYch4Ze%2F&X-Amz-Signature=ce8b3f4cfa8829a7caaa224b33ccaa61a90dd0d390d243f0ce2d4268f5eb4554&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

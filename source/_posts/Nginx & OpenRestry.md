---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R5J3JLLY%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfsz%2FP8IMYDM1S0tVT6w6cThsLfyo1d3VQgbxsvou8AAiB3fzwDELj1HwxhyDuXXBv0PE%2FsfhPPcSZwGdcAQ7cQbyqIBAiw%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQyMhW4Drt%2FENm8CLKtwDOI0p3IRDsd3odfBMU26nMMITG7YGBEr6xDcT2R8GkRMQaDxFee8NwKGoL3I35ulN4npjDROPXAQa%2FlaX4vQHtrB5Yj5jZ3sB%2BfqD9ul%2FDIh1nhSnIKH9%2BGRSXiv8ogXhr1NIYIOTpxCLfSXphRID8Zr3lGZ2BHjhwBdblC4YBi0KuXLQdwxj1TOHEzf9roEHXf0pUGI1pD6XX7NquTOJHZvQhcWQcpT9q0swRFlHAZaFBPJSxbK4WHJ%2F4qGLSpZEONdxGaB%2Bmmq4bNP7JdPhOg4BCHll3E%2BAoyD37EJo%2F9IDhW1yRk4VfmWWIhQ1I9aCG1Dy%2FTnJxKnH7n%2BqxXgge4XfV3L%2BJd2lit%2BQn%2BaXCnvnWgBCEeTThovARUkUUFGkCzuyfIvjjOH4jnTd7%2F%2Bg%2BGaa4941qaXsxzrDtwQp15ySnLfLu%2FdR%2FlvrgGwk0ksruAp9j3CwjJo2hUfPdQZURqkKByaDR6LpFtqJL5cI8mbquPamSbtgPcFGSUvDIkCg0XIEH%2FVEeFycMx%2Buo3Gt0iyofby%2FtHy%2BhqiKvyjA37GFRimDO%2FtYPCB%2Bd9NdnxamGJGgc0Li3qH72xli0kekGBcdfza0UMWgr0uhLMLHT8%2Bg2ujSBEGkssUX5O4w6ursyAY6pgH64S5qRtixcUfQ%2FFGWXDAf1u6jibOv30SrIDq7s2KtWQlwS0FRfE2STYpvthYn7aDJO1jTa7Z%2FBX3WiL%2BcbOrYSXSRtmk%2BUki6niQ2LoWbxYCc%2FH5BNjPZ5FdXAMP1m6t4zhDOq%2B76c2roAgvN03ciS0nb5a9olB5XwffvQfFag12YfoVjubwULqaBC1kFT2McdFU9bUMYzorHiGH8Z99BaQ2xYHRm&X-Amz-Signature=f06019a5d415009c6089ecd5e2e89cfb2a84052b5202acd1acf8c06219380ca6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

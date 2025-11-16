---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6LEGFTD%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvUJhnbLvk3BjeHrzPf59%2FC25wEnT4zaOwCRR5NgLyQAIgQeWk3Fzv%2FvJxaJKSrVbChwagUOQthLhZqcurDChPTkYqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKROUxgH1YdTRX0t5CrcA%2FqTeYg59YOoZwwHzFtk8w213IzBOYfC6cIZNgRc1rb2mkOUHpuD%2BPEWhtHnp4lCN1ap6vbN4no4fkQbxqapELcY7Cvm2FvKQacETVUwOtZuPwHrYIXXDSm996DpQnjcH5V9u7sQsg9EjfimWaYNfD6iNJ4R0vPrI59VYxS6By8WH5eikrxI6xPay3OH0obaJCCrEIF11OGBiyYuxBZzQl%2FH62egfy0u7qHyVQcx7DqVMDN9YZMNM9gsTauDptHXgtu%2FpLCRr5u0YPW%2FQQuFOz8g8seHghua0XeP7zltyd9%2FZHOaYWXqs%2F8OSvrT%2FTv9zKtg9CTXVckUkxN5xGuIp7czrcA2meEVEhl8RuOGJu0di%2Fwp4mIpDwxwtnVPPN97lZKhrqLZq3lfZ3ThaSbM%2B0Pa%2FXTCeTq6pEzJ6jrnvFTPZgfF8afMxEJqeDNEKoXPL0ddmytDKEWR%2BdSTAUt3Al7zogknVZAXGHxxfj0VJS7c3DPtDMy8Vjpxto%2FyeYAqIhmHvyox4Sn7KB%2FOAwQ12D9lJRMV%2F3NEFSXn21nsFWNDK3%2FOfom%2BXBFjLFAknxzTjwius6mlisSPI%2B6X1H2fY6wTm%2Bn3RVBpVlJvSXjzKYJG8savqobtwdkaXo6QMP795cgGOqUBQFfdXt1%2FnPj7yzZqbzHu9tJ4CEuj4PaYYBxHiZjhMuZKoABbPeZUQeUdf7SL1xurza1lTa4BEux03HSx0M2IrNMX2dMoCHLjLMQipUGDTZfrxtx%2F2YEj8FFZ419jqUzeeZfFgWE5oEIAZpUYcJxbXfL5raBV1VhAL2u88rqgtbJgm9ruMT523t7NKh4EOBYMpjP17nt4PggJKd6%2BCRfTLOKganiL&X-Amz-Signature=f2f7cdb27b6d6943e4faaa237749fdcc1fd8af4090ce4f6959b481f62124a2a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SH5FXBX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHMaCXVzLXdlc3QtMiJHMEUCIGyF8wiIgtP2eXTv8AR6KqLyMXTH11%2FXTowe8AQTVQeSAiEAotazN1zaHHB4aK75OplWE9XnMG9xE6VnyBI33Cuji9kq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDLomcy1EAL%2BsURKJlCrcA5kVtXhYIFFOpcnzzdV0%2Fbi9M8EDdAztKLpgBKTZRk6nFt5reb3HK5V5HqG78THWK8oIrr9Sz9LtynsR5TLO29UDWXkeb9QlC8IzRYCuTuIUFL6uFGHPkSaGdX%2BOind0AMKA%2FB2IvycVlz1VVgMxmDiuVwuIXSARAR1H6rn3jULzejcgY%2FTST7xBz%2FRYw3cxkc3%2B0PmWIaLnnje8PQXOKLwfGbF6iOg7S7X4AK464QirdSrXnO9GwbKfSUt0x6ekpDI6Vsej6O99tw8yu7QgO1MN%2Bw%2FgSoRNoZSVKP2w2Gb1NH%2Bp8P29ZV6EI56QbEK6K3mVp484jHH5W7iOEJlGjrKD2bLDQC%2Bx494zktOIH4PI%2F4NGoEhJMVSOkS4nPgIuj3ylT2lOYsZlZU46iBWk3PZ65OV8M%2FSxKcRDQufud%2FSdNCK4cmL%2FwuplZFJo1zHtkMuvKNzJIBj8mxxXgxA6fT6sFwv81VofOt%2Flzm8XBdE8kN9GeIhUQDISuLTmnlilMn1xKRbxld1XdNV9QCcfeUSSWJYm8Kp2LZLcxsAUjEQe1ygtL%2B4Fk5zQRjqOPAIyHLFNnsX1oWbfmGf66BHG%2BjjkM0TM%2BspUgQCpqQjYVWhjDzup4ymkbDC3ikAiMN7JqscGOqUB2uSNB2UQEYyMpiC7QFbDY%2Bol4qzWbZnq24QCtn9PSdXX0GkE3OdfP8xXXBTkT4eVLbMVi3XmH1qrJDNjWg7umJiz1jOtomO7%2FBGGbmrfD01iVYHD66FpiZSSLz6WCGMW2TvypUrHX1HwB6hgvZYNjB%2FuzUGPGBS8NzZQTxjHXTbbssRVzN250AXCGP4P3%2F1rn2GiEOJdYFxoGQ5yiFKozE1Ey3so&X-Amz-Signature=abb17a571d977b03c9d1e78f81bb8ed0cf274c2e1c932c373381f2b2aa24ea9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

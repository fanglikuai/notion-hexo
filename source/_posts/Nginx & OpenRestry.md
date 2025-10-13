---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SH3CWWMW%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T070114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCgP45uYdjnAiadIk%2FPmlswAgXnFoZNtRiWZSVYvzmGGAIhAP2bhXOE7HJwM67VqhJdlO4zKgd3baJ952yJc%2FRaOpw6Kv8DCEAQABoMNjM3NDIzMTgzODA1IgyiWD5bsNWL6C4Yl8cq3AMt%2BDDY%2FtbbqnsY0rM4in81PKAjfuM9DWFCCVYrIr%2FX9rig%2BlGI7jqfSTSwb0MO%2FmDL%2Bx%2FPr3AKoaoL9WVT5vO9tDqIbcwBSWDtwjwmEgHFByYq2y541lk06xHqZsXcXErKVM0uVMcPYB%2FOexdmbgoy1SvFxKbP%2BdXB%2B3Jlw%2Ftpo31arSoEQIwXM82s0F7gwScHHWRrdzWsieZY7iVRxZVOxIeXBTBw%2F4jYg1iN8vdliZO3CeR7OkpLcj1IuPTSIOYLr1SvElBwiUj5ScAVyKlM4iBg4uQaMCKv8muifVx44nZyoEOIdClMfQ0GSIcVudd1jZGGIv2D7aJ%2FEjtMUkEMd3hOZxcseQl0Qors1%2FwzttlIj04%2F23I%2FYjfksTmVo%2B03171KLwsSUW4KF7whxqfkJ8njabxdYgNULLZwiDT0Kb1%2Fo42Fz7Tna1y2d%2FWzewNdyVv%2BtCzEBkl7891HEiZWYgOYszF6CGqMONMN7pL2zB9GNtcemNagoLvBwHzdF4r%2FImfZT1BvKQxuL00cVhtpJOi5hUY%2FvkQWjrhmqCkzW4%2Fp66j8tk3yu%2Bn1J%2ByYd0rW4nGoiCvqOECKclWPoTF6EAhqlMUy97Q452i4AW8d2%2BQNVnU2%2BS5sk%2BWB0DDivbLHBjqkAZm8ldhhUkXceG0bLUrTvdm1fL6aMK5vvuR9rFj63n1hv6GcBYK4Ow3WtRkjuj8pI10vgEWD%2Fslcdp0SPvyatOKIDpZ1sQ1lq3dDqOUiZf0YfW9r1MAkYQ5iA76P0g0MZYf0U6LBHBg0FkKEEEpfl%2F9wkPFOOfcHe1AjrW%2F%2F0fumokBUMR%2BFh2sCV5EwLOc%2FsIcUqZregMjBLs5XrCE7NeCMjZoO&X-Amz-Signature=19a16b7b32c68c9995ceaad2aba52d98108ccdea0492791408478fae8d37ce3c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

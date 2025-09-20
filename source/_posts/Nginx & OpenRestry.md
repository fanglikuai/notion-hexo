---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7FARLRT%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIBeDmklNDxZ8%2FFOUTo%2Bcj5%2F9cVnb460A4OqQxp64GkiDAiBVJKgDnyW%2BuWRp673DxXJSCyfNRU3t0ul%2FYREba3GODiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BsoWh%2FsHzSUuXvnlKtwD8UqnWO7rDqJHwhBXoRkpByDxiwa%2BiyxOelSqyEws%2F7vN%2BZC6rIZA9Za7hIFK5oQmDn0Pua%2FGkzKZN3wRl6y9LPoZ%2Foqk9Clh2TvjkDlTQChX%2FmD3FauyFzHz%2F3OcaITcX9gm%2BMruVi1Rc7WLg1REmcGrqqQGuojr9CWm%2BJJlJTrvHkOXzZvkd9HzslcJItYLnqtV3iLdb%2BwMoHwYQauIdB9x4QlyIBOmyqfO9QumKnS6mAqHg2Voeo6JGwmlGFm4KVOHCcts778wTWh6RlXAMVDdkI4HvOH%2BEzdC7QBvY3Q7i1sT9dbOySPkBIWYFAPWXKs%2BiXLGlko32o65XIL6uNDUbq7KlRtDeFUFQXPxy96OXE87ujVCOxZs8tNrJkCUkkO6mB8Hr6A8MyGbk0M%2FxQrkSkssG2ul4Mx4s4DTKmrkqqLqjE4pbiVt0%2Fj25R4MzPvcSSMg6KgOAiyoisoow7Qauo0xQ5vT900Zxv5vwof%2BB1Q41w21xQQuQMHsL%2F8kit%2FWFva7wQ9jNJlmfP35v9zOKlNX5%2F9mFit5yZS9CaOEV1cm1NvqajHOXF%2BlrF3MAHhy0%2BXFvzDdiL%2F5Y7sFOJe1Yf0O9LOmHcd%2Fg5TPsG5fLIODincD3TEIBKswu8u6xgY6pgHLcVlkAOyKy5YIY0Xwt4f6obIUvwG3I8JjDuuodb%2FNvR8LM7AJUK2A2sdPtPau37kLryBGocp1i2vb8kjg5mwmY9h6fBk38IFfpdbHIiJtAap4eG155iqCF%2FF%2FK00fIeT2kVMlu7aPb%2FxOcrFQ%2BQhZwm1jL8QAGn%2FY%2BKTtmKYFCtirhET5JjNp8Q9VMIxps5vc8EMFtHGYPoSn6H8hTy9hivWFmZfq&X-Amz-Signature=7200e77809de0dbc0460bb478a98562e9d63cb070b607bb5c79b488f92a69eff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

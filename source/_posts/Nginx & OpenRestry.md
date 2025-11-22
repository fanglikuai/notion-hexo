---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QPAAFERV%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T040050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIFtGmvv%2FaM6pnNaS5hw%2FUVv5xajRjqlulXmZqnwCCA01AiEAtWAXgmyc2z4VPIgfqP5m%2F9X4oE2uFxKFHcNZ7iUecMYq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDJXq1H%2Bh5w7AhYbJwircA6JCpv6yEx8kWSw1u0CPzVbEEOs98NRMQKh8%2FmI%2B%2F2coBqx7aU8o8pAlvUyVSn0GfK49%2BRGu353U8Vx8MFUNNlyi6SYe7DCO43K0G6IKQMkF2K32a8H26hiSUfTHdU0ty7JuCG4uAVNheKnN9KLaB5wXCGrVkKuWwoNROqLVCSSuutfp9MluMy6Q5Fa%2BiwjU2SRduREMtAw0g9MiOJGJml2YCllKPwGsOWBTh%2BbUgLS%2F%2FbxmyBIdjVnajesgFWAet0AGNn7ckO34ZUVg1O6PGUDqdTTjeIONlu0UhkZclwsO4TcorhMXB2lM%2Fi9euFqhTX4ChNjzR%2FN3K1MaHPQ1EYAyg4dFESkDwktz3%2BD21MtoPcvgaDGZtDebkrhfnNKwJh2ThjR%2BHnoHHIEeLjXz7xfDUjIl3fiVS%2FOrYN%2FUG3XlPhDuEByHNX8Ep1o3KTXqe52s1OIb4fZB21Z5l3nXwGNXT5tq6eSapBAO5%2B%2BEYD0%2F7u%2FN8FlecAOavMNa3cagDag5Ft%2FUTUHbfa4avja7Bo%2B%2BB6lmLHz1UA11L2W4oae8fQryiyYMgeH4Wjhkyhwz51xDoXQPNdgFjTKOW73zvEzGNUJ678hKGPzMji9knJoH34YQrd%2BaFWw5NWB8MKDmhMkGOqUBvqzdL9U7m9vyqnB%2FYo84CN35X5brU%2B%2Fo1O8tzUZi2tGCpSXYdlztK8RZYyjInJMT7gvTpV3YP6CNFG9yq%2FkZ1%2FlfsdjR8nQYLIH3WGVQrNy98vnJfz0rKa0X2OYxs1u6gW28a7V9%2FX%2FXM7WEHfhe9HvmucxB0HqPRZENQxbmwEtqp5Evv51FiM7bznrp%2Fe%2FM%2BQKzWQcWF%2FFI4Rf1CpazWeVPslnm&X-Amz-Signature=64c5d12d8ede8c4f161d47a8ac3a3a546ed4410d192afc378717b453a23a207e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

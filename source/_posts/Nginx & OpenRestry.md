---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJ6OOA64%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJIMEYCIQDd1sGhjf%2FUlujCUEDjOyyqZfFOmHaguxGTKeAyKhawOwIhAMb%2FydYA%2Fs%2BJSaTfo4hArtea5hWnTiOHFWyCpZvLxPpPKogECNT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxA5DL7ytMg8UL%2FQbIq3ANmSpeRAVQ%2BrsJj3FYcWCiGdY3ce73dbk2itohfLLY7bUE9du%2F%2B6lgdq%2Bls7qCUYCCvaGsQikSOTB8Y11Gs7%2B67ZcqIds6o2ODDEAcuBXr6%2FJYnDtBqh0al6Rbfhy4JH%2BpEMkbYRDXOMDOACR1AgPoAYWfsZyxzdDLJd%2BWwGNGVNEq%2BpYPG5QV6J99b2m%2F1U3GBBjHogWCKjYnawRWY17dlRQM1QpZTAIiiCuuD6kCDj%2BzZO%2Fkjrw7nAnA71avC5jADitzy073aBA49WTRmJjh5pH0Odw44lixISAD3HfVTYEXDY%2F2CwwQno8DsnmVw7y%2FAl7LGqJ55x6QmM5q%2BQfpRX0VGWO%2B08ZHxs79UlU4T1JxfW2GWQk1Opa8G0BY0dRUP9BZcrd38pfRxpQi8wdy7zbScExhxEZdA%2FasVtEl%2F2QLamc3L1CVH%2B3sZvzaXhi3magH6BwVFgolQk9Wr9DUzyXRdgMt4LzAwD3t4VPORhpOgaEDvPHq9IWXCddNaqLjgBlg3Jp3E4gPVD4bu03n8u6DrIg80B8II5j2ttm12%2BCW5JWftIyMQNmBlxm2%2BNgry8lEOgwz1Lii0xgHMmF4exy9m%2F7cRsxoPMVD1w0daBLd53LhtgOlIq1OGdzCz1unGBjqkAQbab4%2Fuw1lZXfcc7BeciT63zX69F%2BtlToF6H2OyiXewtLBB%2FgDfqP0sbskmTC7of%2FxWiniberpdMuAPWyQxYFZ2SBXXF%2FjRu3cUMcdeuWdH%2FXV5EWyrS%2Fyu1aGA%2FUbjURW%2BxzIkkCmRyl1DFfhFh62fPLbT3ppkS%2BWylcH3bq96brl5OrIGSNS7UDXGvR4wHLc0Eknpbs2f944JUMdSczet%2FOD5&X-Amz-Signature=f214f087b800f3c74fa13281905035cebd94d2715f365b2855d8b2689179337c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

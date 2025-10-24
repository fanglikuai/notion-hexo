---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRAIOWHM%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T070058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDPzCd9FS%2FQaY9UfKCsS%2FqNHOYEEOCPYds8VWavfYJvDQIgKhIgmtHx8rvFsGauqBvBzJPPOPqRFvFQBsJ1CTxWOKEq%2FwMIVxAAGgw2Mzc0MjMxODM4MDUiDGnWQTEBLwgZjgKi7SrcA34M%2FYpvQqDQHtuLNfBcboO6EhFNgZe%2F9xm2tTsoNG1521gtGVxBhwfN3Sbf%2FnrPNuuOzh%2Bs9CCZDqGSCYtomAhdor9LPMJxSZorU7GWkQMZFhkbn9FpMjdpnNycKeaD%2FPnSnHMVhRXWtUPcvP4RKVdcPbmtHlc3mfxDKJ9pofrUaIEBk8xuKJuAVJ9LspDEnhBVEipiMHWGg78E7CI7cVuaukGs3V9qmx7mptLCscy15iR4bEZ609icBEmt4JS0SKJMRNhMnEgu9RhbXsrcO9ysLi24olwueO%2FnJRKVbX8e5kuoBFpGAXLa8PyQn%2BlZobetX0aJ0CXdej9bFlyff0TLNAdJvpfcacmSjGGqRnTK7fyOdljaOFaZrLR%2FUH5%2F8aVAcbJTi8%2B3RkCdjKQoyNlzPkMLp17K3zt0JFoH9EmzPiSCYJ7OMkcfCvmMOrCSlPbDZGil3cngRaEIwtDP3gOh5jXVbMAM%2FqkDyFQQo0Rj1vyT%2B4L66YSB%2FgL6%2Be2e0V%2BItfBGH8MhY77MGIj8iCJ3HrP87BnmNJIRDSTuJbU2oY6BKZrtxWudK3BLiOvfjG%2FCtYMZ%2Fjy2EfwFiOyrqo8lGNN58POw2gJ4OI9YO0Cc1GUf2L7j16lkoFFnMN2s7McGOqUBHa8GVCJa5172mouCRp8KcDS%2BB6eeWOBrwSwbuWb%2B2BGGPUwnco%2FmGHOevWCiDiqTltIOiUnp9DqQ6rf1crrH%2FCZt6MOBonSMcLH16mv6FD1vgt83Inzc%2Fvbu%2FBrKIzubI0%2F9Xa2%2F9MgN4owxmiNaYZWhLTjbFscxasDmMB4vVYUhze2rC%2BocmTC6VNKnyntHKadWblJFNkVu7p8BQFz0ApO%2B1nem&X-Amz-Signature=16334fe5f6d3facef7ed583ab91d2ae796bd4a4d46c2f972bdd5ef5f856cdeaf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNBJX7FA%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T150040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD6uwOtUAWYwa5yULVpaWIT8O5m%2FjTuntcxN59wTMV0mAIgOBWhFT4basiqh%2Bs5YXl2qnIgHGuUIqeTVGpA744y%2BqQq%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDHHl1MjHHxXawefq1ircA%2Bn5Kj3g6CbQgF1DPrsFrnsFcqT9sVoVtqsXhDQaYbZHvB7ySYJEUHzjxs7YNN6eiSMzd2wDZ0%2FTKm3DwSftX4pQXI69ytgPo%2BsXRRhDSqMAlSuvpCSWIpJ1EB1hdQvWUpH%2FjxoTrb24XbFxDuPyR0uKtvlFQ5gxHox81KEyOwYSplMJjjPpCsWUb2vg9Vv1Hty3Y0V69NhrRoOJbI8aPqfyKRPrQcwDwT3c7lOu9s3XCxLuWCKF1GVWk5RjUwfDp0DoDO52wXj60PDm4eZqGF9CvIRDGxIPWG5uhhewJCKHZ5dwMihD2oxTPs%2B380Bx9jXUK1ii8aBTYxWkmIxjGC8I4sotgRSqVqprfn3Pfox8%2BBk4h1StfvoYmnLM%2FoDbM6F9Vm2k1qC5tw%2FIAZB0ifY9JosCXJQl1hrqvgtiC09l3OlIvQ0HHXNIajnyeYKtVdouMxwmIBSaUKebRZXYyh87OnG9eUcC6lUWfANyFbH%2BmaZA2F53rVi8zWnjtGl4DocH9Ov5o8pTR5SgOqjZnARo2WB4B0RTSzHuqUfThGTw%2F1NVxQD4OH7n1u8oKi%2FNzCGOkRselRQ223Ybol4wi85DG%2Fcesvuf%2BKZaBCb9dcJ3QCxI%2Fvk6kHyptqmwMM24uccGOqUBFOaS%2FV9PCXD1zRJytDYcc9lt9qk%2F3%2FwyN%2FFrCeHR7x69eGgvc0ItvwVJRcG6uORifkA37x%2BpRXyytfPdVx4tX%2FF0xGD1li8%2FQeizvJYkhWR9Xyhk9X0jPNy93BVSiqT6T7iYkajxRFmPmfK2Bd%2B%2Fyb4dnNnWvbGlgr7CbHuKh%2BwMmFJEPU3d4aqF7AqMsu05m%2B92OtRi24lQVnAzhojLtWLAPSCz&X-Amz-Signature=068105b20a044ce41852aba5b4e855de00b7627dc2315416c311743bec7cd67c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

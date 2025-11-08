---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662CVH57MM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBIaCXVzLXdlc3QtMiJIMEYCIQChWiDYVX2Fus9lcbLQFO1eMAvMQhfhM6bjWfYfZ4JRGAIhAIOWCVOQY4snrhnutUO4%2FVx0NaW66MnlQdGoM08%2FzqCaKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyEDtGVp4nti21zpOEq3AMZ09Cp4%2FWn6rno7HTS7%2Bx5gici5oxB8KlEy%2BpsTguOD12qHKpyoQ7qBl%2BT1ldy7NNgFziZQDTtIIo5pFxGdx%2BScrS0SvgQQpusjbbGD1QPJYQF4JS6nkwHapDwnu4QfY2wq%2BkXkcG4fkZ1lqBprHdd6iyHiJvuQw5HnlDOKZZSm3zWToenj97SCI3ndaCfEMNyn5zTqvLckgho%2BXVsXKIqWKpMQYrjIvGz224j03MSpH9B%2F4aLWuBEhHtbsPFIvw7piO1Nz1pvfbFxzlCvZqvA%2ByfogCcArZsHNyPXi%2BBIUjMx3StYPwJAVIl1a5H6FueVMQpXfyGz7oMnKGdaiadmhXK0bI6E7h100geD%2F2OF0Kp0JMdS4ftdV%2Fqu3rUFJ17OtSw1btD2vppysJLS8zuu4OGHbX3rOTbYkj5hRC99ObyLFvw8n0xM%2B%2FSxn0vkZFTSNzt%2BYAdtwnl%2FjIpu9tMSAx%2FV9LaT%2Fb5%2FFZkixpWZ58R9QAWr%2FR2ALpIve4Ffzct4LL6T3UBjt%2FLKkjz6Z1xboj%2BGVPhTue21cphBEKyNEs9BaNpNZHr%2BmdGurmjKtuTwK%2BylmmhhIqg%2BXODx7cHWUnRepWGSOr%2BH%2B4LS6fRlFeJdFRczgiaWPWFUxDD2%2FL3IBjqkAVRHE%2FdtcoWpOLSHTTFtj%2FVo4gD53BSUkf2qSSmtExXvkWQyHWEOCd8463QZHcTvHNO4hemF%2FO47fvpO2jsshF6XOH%2F5U1voZ3SqLjljL4pmqIMSbFopeAb2CeXmwbQcRlUlPS8z5Bluk57ALy9f4x4pcyUECqd8DvMVADjkHqXSy%2B6BH8EpWVspHttyV7UpWcbGjEo9HydLVOEXyoXDDJ4XyjQw&X-Amz-Signature=2f8f19f7bc8ccc11e4f6098f19735d4b0b75a7f2ac6f275ca81e96a03d9989f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

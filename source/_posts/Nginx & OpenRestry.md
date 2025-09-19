---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBQRPIEE%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T000051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDLzo1jqRfP1uBl6nodE%2B%2F4GqoHqkcCxh47764Rtl8VIQIhANV0SsFyovG4%2FDNQ9cfT4poVnJk3jt7chwehaQKpj%2BJ2KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyIPWS%2BeKOgQpXoZ7Yq3APTCp0JxAmVF2%2B5kWr8NIGCMXBMORWwxFcrqLt5GbNWlr%2B2%2Fys2hGr0rx0ZVcBAWzcpLkU2yPgdVqJ237FE59kVLwp4RD1mK8iGUQHaUfGLffuh22yefZX1EAk%2BP97KjYXGDP8i99Cfp3l8oM8TNNLWTJF6PRVO6OnlM1AmDmqzkwQBoHbSz2UriZ%2BYI2fhkz%2FLnIjgoEwfDYIghXVPIbHshVCprkL1yABZz6IVU4pEVM7IZNvDr3soiiL%2FDeNttL%2B9o6Tj1aaTvYpyRg0L2DzzNrQegctQnJqcu9nDWzdiw7KGXHJeYlETOQlx3jQcVUPz%2FDuKuAkvEw9Qz%2BCkIGm8VtItmI4XWdcpL211dRKJR7R%2B4u9QWldXGZ9DG5dbIoiV4T5zVaQ5ED%2FcaH%2BjIKTF8rM6rsQLsSDRmIHGUx85p26AmRfoc31Dx2mifnuKJatrLVCTdJnldB1Lq87e9%2BS67y8WJezOWKCh3lRK9c1Aacae0WQvNIWW0MY9i1f2VWidaPlXVHLK%2BQLAiiQtvmoKb3vWYw1lJ%2BNMjTfDJoH4wEIFtStVVgWhn7PDzNGC87m%2FWuxnwSHFRU7iKCMWyvZ%2BKXXHjHSY8qnsPn8m7Ki4qLYX6bvTCBpisNAwSTCDmrLGBjqkASeFHwdqnamOfInDrCyYQUHCyJBAl5009GZxCGW8UJiIg7KqOXB77GIgio8chhNbTpGEggRHAAgtzvNRcfhBvJ8dNELllrndmgN883b7qskYETIgHMIzcJMJdZ0nqviuKyFHfKYITXChsslsk9lNI%2FnwNEE%2BP5Z2vBClXaPakpLQTbgkBX5CsgnSCPMJ%2F25DnfNOYK5BHZIJV3Bn3IyycK3%2Fw4wW&X-Amz-Signature=b6ca5cc055a1d285348b21a6c0cda4335fd4d1c7d229fe70328f9701d1c48edb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

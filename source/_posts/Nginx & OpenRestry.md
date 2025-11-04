---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UUEJP525%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIACWTek9%2BjOuX2CgoZtXSLqSg%2Fl5DtJNzCM4z6c3D9w0AiA6j3KgUWYCCQlssDCtdTNG8yuOGTH0yRGPzrfKO8ZzPir%2FAwh5EAAaDDYzNzQyMzE4MzgwNSIMndUqGPujPjgATe4vKtwDvLClWVzgw7tU0sD3Cs9NQE9zUEjPEH7h2riWXmP%2B4%2FOzpcG4yMTEHcNA%2FuSBKFcaFeZTl3I37Ljwo%2B3LHq33tz2zSvv14%2F1jrweQUxrk5NJpPc2ra4FC2ASecDM2xbeQ%2FyAJN46PzFSwSkJyFkOfvKnAwo7QuSn2aXsuW8aCR%2BWFjnRBmikhwp7cl8%2FpTtC0f4UcmBlrU2IJGuDLuyias2GBqrMZxG5riBEEz4UAD6EmI2%2FGk9It5DaW4jqokenS%2F8OZOZsZzBjBvau%2FOA2%2BxArY1QCRA2kTvfvcVTg9orpbspgKjTnHZuQef3Z9DiiIP6Y92eZb7%2FsXMGPvr6fsvY8jVLH8Vl6W4k5cWX%2BwG0tHsGPy%2Fwsz8haRady1TTdqjtOWvM9trWGwVD5vJi83RRxgfoj%2BA5ThWWz%2BHa5YOJ5UHsyX71onrJhC7ATbDHwjADinsDHgMVF4DD62XJy%2BNDFPb8t37OasWR1gqlqy0C20PVUIg1P0w1MVQfcltrP9zldiejia5DGeFhNEyBfuQakfv0MFSeO0aXjZFZ4APFFVOnSj3ou5wzuIFcQVD%2F8hqfYzRm2yJyVofW2k%2FGdaelEISWnEt7ObdvxvRhCuRePq0JF2sjQRjIDUESQwwLmoyAY6pgFJz%2BTp1CkZVO8JTTyXg6TwVOQ5ZG4AfCWk3YGNLVTQf8907c%2BnSbVGIcar9Ab5KBIjv%2F7xidALWXfN3CUkzGkOCUv%2BoWZQPvDtezO4sA1kIhqHYgYzy7wxmu1EoU2wbVF%2Bom75YyXYp3Vp6w4vdIRSl1AwrkM08v1o0eSu5wa0wTdpTcqMGJy9lQzAwtqIKXD3zTxED2iZyGkMJ%2B%2FCdwbSSHkCvrda&X-Amz-Signature=9c1badbb82245790af0d582731079eda7fb8ec2e0f81ea82eee0aaedc66914b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

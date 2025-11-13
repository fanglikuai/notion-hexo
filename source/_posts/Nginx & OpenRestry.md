---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VI2HNJB%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQC4TI0uKMo1JwoGI%2F738x1kw52BihDnR8jGWyotzAXmwAIhAMgn8WuniM7AlBBI5zOLa05ailwtwpzd72QKqSlZpVK4Kv8DCEMQABoMNjM3NDIzMTgzODA1Igyu8aIEm1XrKLUCu64q3AM9KdOrtp5AzfXMsh3XdYPKnB6fuR5HFp93QNoWkBJe3ffdln3tb4HV0IYpz5e8l%2FZD5mIzJG4kWp9ADYoXqp6XM5hjS8LRP3zBZOUL9V6C5DV%2FvvxnShLzitgT%2B3WzQgJBJ%2BP67oXuWn%2BOD9Z%2Bwl2fX9iispTpB7Z%2FygI0l%2FhnwxNzhW6VXlGrxRFpkYyFSWoPKm20EHJTj1EKpyIyCw8dNR5CpScDTU3tskiBmBkHIItIgloxoSOTF3vrsqXQh1NYOw82HqCT9TQvqq5UaXAeA4D12%2F7s33fWA%2BtOc0vK9F9fkHOxJUvXS%2FGPwlk9PoyPe8cr5egF1Yf8URfXpOD%2FsHAz2L6UHt7UXHiPo%2FWpdCBVcGtGgNYsmGDjMvJk4J%2BArAiyHK4r3hqsjS3BPA8zl3%2FZXwmFL0iewFdfAZnDH%2BbQtAI365kzmcm3dVuXO6R12a%2FNEmiRC%2BvbjsuvAFjrV1mrHVQqnZvWR%2BpqRlCz94vNwof%2BndBsNQNDK7sz9ku8tLKzLoJMXKMSCebnxa0goTCMqw77hVYxnnEKgJTd8%2Fg%2BkjkJg10Z2gghEnajYeFPmGxOae2kp6bxsGraI1P1EnaSIUxdv2RUgZPkinvnLwepPuUcpesMAR422zCJ%2FNTIBjqkAXYPy1cpvN964iznqCJiJtOjzNs4l6d5UhJjuVtNm4Pj2ki3PmcWxrERSIY9JEombeSp1Yv3bp9CSx6Vm79SkGtBK0IhLMxRPNZCJA4WUtdGYZkH6y1FdtvuX3GwlSF9k3kNPjWnz7OkpmCOTUt%2BEsX7teMbbz40Ve%2BX72475DivB92UeZ%2B2dKHPDp0g5KFykAWgM49ALkMyRqAbynyjDf1loLnz&X-Amz-Signature=4c90b973424e35e9661cc9c7f10cc27740ade86411243ea630bdc8d84f1ebc27&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

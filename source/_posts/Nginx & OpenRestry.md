---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6VIRAW7%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDKtpui51roPUQj0TkQ4aSwWr%2FixxaBKZbanClsvPwbsAiEAxAOPdMpWlVbgAg27ZHMPqMlJAtPPfNL5CfqaDEH4Dicq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDAopsQTTnXcVQiFjzSrcA9JyVmeSoelEYL2OUxtHTRFutnlJ%2FO4785dI3TYghYGD7iI0cJHEmIn%2BW5PUXBgFHI8DuuT12NYyyhBR7QYJpNpR3QGAbQG6LU5k1AvTevU43epO2dpaq42kfdLGe3xKT%2BoZi25CIOtFiB0vWKPk98f0abp8uunViEK%2BF65n6UnEeoXmOlP0p8mnHICbWggCQzLDAY25CTPEURs5EcUwHn%2B4bnJa9EWKHexV3r04PwamZAkWVH4gl%2BDWaxWz%2Fpzf4DOv1OOrgyLcTd1%2B81hEe6GKvvNs27om%2FccEvi9WwIJRO0D9mRL30Zr4V3BvAV4x0OigPSZKBHtMKA9ESRdrTWTDcb19ZU8WLra%2F5Z2Xd5FO4Vr7IiiqwpAFEDAtDs7uKfxGFmbsz%2BAwcgp%2FUK0CrMcHecbpULHB%2F8Nj7StLcvcuzN3zf6SZHjN6UP0Hu3FPjdbG4Jwj%2BYbgqNRRu1KK7mqYNN7ne7wDTN%2FfxfAApT9OcmKFWv%2FFsKhTfwGa%2FV86uZn1XtlqfS1Az7Z3WhaMdOXTcfcHpJlbQOCTEPMLuxgT2lVp4ZhM0hlJaxBY6%2BRJSa4XYXIMhBeQjQCPlpWZJ2BxgCXznbfaQ71gm4PHLJ8X1WOJJfK6JYjEp6pkMMj0n8gGOqUB4HiQJzve1Jljelu5aitP5K44bYp8aitc6NL90ZVxrDWKqFbpW1IH6IjGPNVT2PfEL5LTwB8f4OWJ%2FP7iQ4kntj%2BbfDHI20%2Bnt0rS4EOn58YVyXdsPtxBjJtNCwPmBRTieM9WX%2ByBhoMjqQseoxUfqjYyhYFW%2Fi%2FMwqkpYZvJoQyWUDE7tEzL3RFipWHXuOCtdM9R8RCgNRXXeh2%2Badf%2FsLtSfcW%2F&X-Amz-Signature=d51a7a953926c2c0e8c4120f3f2f1c1268af4861822c10cc1f52dbeae3c67222&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

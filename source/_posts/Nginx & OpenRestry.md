---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7XCIQMH%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFgaCXVzLXdlc3QtMiJIMEYCIQDvwwXWf8jAmylaRwqLq302l06fpcsGJXpS3kg9%2FyQ9IgIhAJRQ8qbBrtB%2BBSNRzYYHCpcjE5dHPjyQb5vymhVQFN50Kv8DCCEQABoMNjM3NDIzMTgzODA1Igx%2FAeiJWQ8sZsZ9yMsq3AOA8Uk7XoPp6izdnCWFU5TMd43Yn%2BkPRjbq5fg8RzIibBSgA6%2FHKcGWOQRdLXgPouWt9V6lvACYOErc%2FwWNRDBOCr1p6mIvOYPnORB2EE%2F9RR9N892OZ7fXBJCOzEkJTLvChFYdPK0m595PxPvJxa%2F7xQhcm5Z57PCkvCVGdtxGOOwk4PoZpderG1wLJ7qVYuEj74UjcQ0tS%2BrM8iM61ADLW0Fe0T4Mf7ApLixxtOHBpBxWA%2Brux1N7pIbMVWzM8m1DeazRniCaWP0QfIR1m1MDzSofoishksGdR2LerJ0dbOGjA1Y9NmLhHcQLMpTLw3%2B0X0a9EafSaukgpkydLycDdpIs0%2FP8B8Z8IRUah5moDHpYerqSydNDWdp0bO4gmfrx3acdLDdyZNtZOxyJ7vzQGyz56VsOnpl%2Bbp0gA8yhLjs630vPuvF1sPXnZbS0D0vyG2rx67sLTaHh95Jy789%2F2Tb6JowkXuPW3HxnegfETP1IjJur1o%2BTSrDGuk5UmqsJcoCxvj3gA9IJj986AJ9eyZc%2B%2FkmmQ%2BIMqVqXEFkkvmRxkifiJiq1VqXUoPJlHc8MakzIWWy2gpb2tria0W8KRp3mpCczM0ooEGgdOuACtzWYX6I77Zo8VG2PazDh3YXJBjqkAYSBduUS1gk%2Fvc07LbAJGmsZPVRlKbRwNxs0WoudkMC%2B2n8Yn1vvOYWbgEyrEGp9nFs8amgzJU%2BqbTKMlZHJIE9A5U12sldsJyvkwn%2FoCZHN%2F2Zp1syVi8LdYLevIuOMhjX9YQ%2BCs4DuE4bgahkm4QWQbpx77ips3h5LNY%2FHSKMqG1SRUILEIQLwHvgm1uX1waSLH4uzxEPVCOad6vpfDzdJgjAV&X-Amz-Signature=c8fdcc450c89d3212f313fa1ed29cb1327647d9e5f6abf327fc053656f197d3a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

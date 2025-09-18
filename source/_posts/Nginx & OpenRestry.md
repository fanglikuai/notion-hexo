---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V2HENSDD%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIHLooJL39329dx3oYq8GvPUxVhOANeQOtwiMg2%2FKOlUVAiEA8Ozt2XS3ilnGOJA9iaPYoyh7OpQArZMXC%2FgZihQhCSgqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIj9zeSniFdUMPkGsyrcA%2B0EwhIQU3yejOGnZp6ce23UgG397EWwDk8fmFWL7BsjbahZhPwjtrI5VlqqNhLcerInd90S47Z76Ld5s6Rrf0UkUBWu4W4jgxy4ftoLimgEz%2FQRelteifOLFZVxfcu8wEJOmGCaMqqsVZU9Xo2z9wC8DObiO6q%2Fyh64q%2FiC41ZRnAiK0gBjo38JbcL71Hll245QhFklduYEcqVpNy9xECfoN%2BP0lgbVE57nXy8xduGLguYDCA%2Bb0pGs93EERxW3kgmcGJs78SyX7piswNzLb7fkeSWwHy0OHpWPZI62rjDVFmmm7c0%2BoCWvgR0ZOH1wSwUQWLL%2BK8h1c5XX4n5lRyxe9mNSSTQ72SkB%2FNo9b5JzAiyGMKvfedTb%2BVDYUu09mbOLqqQ%2B2uBC354DgI1vT%2F%2BH1mtqh4EchCVV326eLhz%2FX%2BA87ju1xCZgA9U1bQ2CBU2BueYv9dvrm9lJDDV%2FUhc50GyNlpAo7YqW3WyX2BWj%2Bterl278C0%2FTKzeDKQOCYWsRdWK572oIIomIzQLALzJ73jNSnnOWdeJp9nFwXe9NaK3mv7htta3SbjZXCU93dTnERkI%2F010KFA%2BK2tCPEsny2LT7YEzS6T3ENVF%2B1Gk0ow%2BwS%2FMBbKFZvgVyML35sMYGOqUB6v%2Fck2dX4xpa2aGQugnqrjtyry0qkXzds8jM6WzEtuTyZsgAp54o9sfIQnCpzrO28unf%2FrBZL1l1%2Ft1PzJvSnQXtf%2BdYSc8Uqva2WJdyvCcPpfyNE4zyYdST77VEmCLGYCu6uNvvACSR8BfVI8OcUZsfd6bxJc6fku1Oz2oS9aqXXF0hr0qAQ7AxNiQTHD2KNIrx03o%2BlhhNSMHHs52l1SJLQTtS&X-Amz-Signature=e17a724d43d1c31ffdc7577292f71484c19f4c983583d664847c73f55eccb304&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664HPCDZKG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQCy55CIC%2BklDTmtv57rWgAQqGKgKewKHdA2vf5S4Ri0JQIhAJEEDnUSl1Vv7Qkt%2BXH5TWDc%2FhVgXB6Z4DOiKZvVETTsKogECOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwvzTlH%2FvH9nmdXAMMq3AM%2FDay58yeKU2jgQvClqkB7Cz%2FaULetnQG9dYQ9NK01msnWzI1XYZApUxF19ROux8NH7TSiFc1QYG8MiAD1cXnzHEgPy0AHHYb%2FlnE5E%2BUu1ImZDKx9vPe6zxc36Ln%2BfxczzJrJfvevVrB3UstJtayTug7IyuQrYpG56bAIWLQflp%2FXeOLiNeSpLz1EtTTm9I8WL%2FjCYeTtyU1Zog7adQJs1ZmjMYRg8vxmtUbxxE4UzK3HgiAe3D4VNDkhTcK%2BL9OLchNIXxpsp3Ygwaz95HLiSXNztGqzkjLu318I4aalVscyVtVZCb%2FhSHzSc6Wy6j1Wq1YiNjOylJw%2FEpkvvIqTW%2FBfKq0DGUGL9LWz5qO8fcPoqWLUFp4ElA%2FEdhyTgTTGFxWYL4YF1bXt%2F1vHefvu6WgV9WeZ2GaErO3Y4i4CWfvJHaovSwEtxDaI41Qxi5sEBf8ptTjzeR4j6rp%2FVOpt2h3T6L2tIk5s%2FVfo%2F78O4L9fio3Hx3NS4d3i8AIP48VXkXBzFBvIFfz%2FrN%2BkosuwhAtVcCGaVNzeACsNDydL5o2mOUwi6mUbrEf4G6LRhMKqPtiruf2DlJBb8N4Eh2ay%2BEPOr0ElFTmKqcmungbGU5wa94hG25K4kHfNODCYyrjGBjqkAXCijC1Qg%2FoNzGLaOzvYqC%2FIKyquvvBBxEDW4maOa9FJF%2Fzh8TxKxvY5geVpIzRo1lK6PIhHftMP7g0MCXzuSWuuHRkzXMP7tuUJRF0oQ9X%2FIdnVd6QByhOmWgAPMwpOUObTOC72idMjwmhcHoH5eiIvG8FvAmMXKYI5DIiVQajWF6yGTmxfHiszQpcOUIjLL6eDoOjbRKU9eHZNO2s9Pa12xDPa&X-Amz-Signature=35b136178dd04afae80af993f22c65c6faf56a796e016656978ca357618ef394&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

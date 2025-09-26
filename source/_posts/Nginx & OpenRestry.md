---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXNIA4TK%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCZQCVHVLOxubSEWSdr62e7%2BvydJWfE5oX44n5qZI4DlgIhAKrwBF5zTQvrgT%2F15hfwiRL4ysNjOklCknyZgdhHHLDmKogECIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgycS%2B%2BXwVxvWzXwBK0q3AO%2Brwbs5RcSfMtEif0Bi8AfiGnaBs%2Fzn0l1ZFCubMU3XdBqx1ablKik0o915IcXPdFW4o%2Bqk359KDdi5PlJ7hHVuflX9feMD2eogMNMUY0%2F8pAXgIl9ByK6cJZYbQvSjbf8xG%2F4E5K4bjA4ZwY8vT4zmu7s5VKUgJal1gXDUBfl63J9efnbXmoAlwTk85YDQiP5Fz9%2B2I4vSjWuanerOP1HleaKNcmYbLyPKkRKpCTP3E2%2FkqFa6hUVo6BGGMhutOCnhCoPaJwAduVhEE%2BzYAWcd6i6szmp4EiV61J4Ufpfsn2kAX3UtZBQtDYd5Gf5FX8ponbcU5jAzlgDSdWfwf8YVEOr1U3EKRrbJq0K3WDNHp%2BpAsyHsALwnmo7ae0nu%2FB4KHx4yGL%2FbO%2B%2ByhDK9LMHMa1Pzvmne88dJsJNeYwWPjLTOiSkp94vlI8fPwp%2BpIKxmfh%2BpMhSaJUuCTH53lGSKsVH9vIBCo958G0VM%2FCRXny1Bp%2Fm6SdQTpBEHiSImIUa8Eck6N2w3MbQ%2FTm7wNRy1qte9OUd4auRvh3pfnwxuLk%2FJ1cakEwqbYEWXevRlgNNN0IXo1HYvuV0GPVU6Q5JAw4zv5KJVtIwAjtzoXam5kqBMTWiZNGYvqtm8DCI3tnGBjqkAbaZxmUKn1E2sS%2F%2Fa6yxWqqRui2y9Vv4XLy%2BMuDNXb3X%2BvqR02izg5uDbmX1nWvcfPLJ08iYRuIXhKIfhOTPM1qQkj0%2FLtQ810OkMg%2B7qOaERwtvITUGpqEQSHwWLvsYwMvkmYui7xNTWYOYgYKURzB4y979qy8KLhpyDIbGBod9h9YlKwoRw5VkRYFH194bWiIZktWP7Jp8Lc%2B72gk2vcSXNK0H&X-Amz-Signature=211a4410f775d9eadef1c77f65f74ace93bc045bebf4c0d30d4ccc28d94ac7fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

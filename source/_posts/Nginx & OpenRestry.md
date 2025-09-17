---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R4QFUCH%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJHMEUCIGh93wd4EAyu2NZ%2FuzSAsqOBXiLeQVn4NQrVwDmBIHYwAiEAvEkSbIXHbRAESOwPNAGoIyXBqaawXeG2hUhxp20dgiQqiAQIqv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIXpRCG1Xz62nrPdRircA2esSEc3zhq%2F%2BxJpBZU8CQYl3FNFC%2BaXN0Rrwf6WuAJ2qvDLVGUZoKIsjWv5%2FaTle%2B%2FcDBqbCMc%2FThKQ%2BdReMJrss2wpGLI7it%2FmQKU31zQ9Gso3G%2FBYA4CmrWJcR0GjfbuNSJASrzK81IgadyaTtblkhVXTAhFmFf2jLBhKA0wTebHPl9yxf8xwKhHXMi%2FelSsl2UU5%2BmwwPw1%2FeQNWEnqSp1G6FWdVcSujjkIFytsLDrfOkTLrEqq%2BuQn8ahuRSdxP9JQsLleB0rhiwOLsAE2Kxlj0L6OXtfB9I9u698fL02fYPqlEat28noV%2BSeaRIeuCKoXKz%2BIskUl6NWMcDgorXJQgOzCcQFirFNdSseV6jNSt1p%2BE%2B5eU2Jm34111vpl2h%2BniNAYZ1cR3%2F4HA5LwfQ8u9wcUsCMUYQNECKMli%2BHGDxabrFKPPL6OMq%2BNsgBpGeWqaBJO9UrkYyxhY3oHYpi8eRZJxxKx7tsabYA9rzg9I8mKBVlL4FPP7TU6RIkTNEVWzuaMPs2ezKWg4JvBYNdgJngW7yDyiV4LF%2BjyJOdJV4LbgV9XAkYUL4Z9RA6BRELvGCnE%2FyJF7lKp93H9upWUAmSNfHZcuAMrnBcHhBqoVsgE74VkYrPu0MJnZq8YGOqUBfMvhRww0LLsLfWGocs1f64j6jG26g0j1vTriZr7cm8TdIztZzsAepy%2F8ZC0ClHkKDkdA%2Bu3hUqZW3iDk0OSXfd%2BjEBsOlOHY%2Bg%2B2RRPsa9ZB%2B3rcJPO0%2FjIQRuRNyxo7tMqEVvkcvxDWZBV97z8dLueRUkC3t507iA4UjONJyPv9ofP0pa5JBgVCgDfCXWnVT7JfYMQelu2MqUKtpEY7WLpWAF8y&X-Amz-Signature=a9e486b5068cc868992f22d87d3c2427ef52e8373bf22ef7f42f2e1035729086&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

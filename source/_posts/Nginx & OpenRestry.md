---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PKZXDGC%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIErjMkwTodcjcIXg3HEaiL8BY9QLIR3MhK4hGuyr1BU0AiAKXNuAwuHZCKFFqHqr0TKtkY1gsCO%2FXS8N9mAh%2BfwSBSr%2FAwgkEAAaDDYzNzQyMzE4MzgwNSIMGcLHRHto5OSRF4tGKtwD3WEdk60Hk%2BFCcm21KgHgL4x2dYTMPNxixFe2zXlK3Cu7RULwo8s6ZSgA%2FxV7iN8nnz64tQf7fsavUg8JZT3DOVCapGaLSo3d8HEpjX5zsUpd0Jeg75Eo7vvqQhRKfxnpMUc1uHkBc3JsdGf0nfhRtWZWQjcyaXZKv7QWPSZ%2BH2ZVza0hlhlfk6Qo6%2Fox7Zcz8VDAwwd3rNfiAyqrs1Hr2LxizFrrRNic8%2FoFYF51vGK3BEU91iOHSpGv6tDsWpSxu8CL3Fxs1sBVISED6JJNpNyBBPvLo4LiNIWR0FICeryFYFlhPZ89h1%2FwAY9uCp6AbMGexqaGdaUclNwUT%2B8mjsFPjulX94XvJ8Oh4bjtKDnKfTJP7DzyzXSVzlIvqUVF4ywEnJe7HfCI5RpO%2F%2BSEyyEpgJ3ZUPuHLV3Ll43Uwln4caYbcVFtpWSn9IVQTsz8bLJKASVJAXi7O0GnFbbJ9Q8VtCleAOjwLp%2FJLaJQT0B0IGG8WGOwyWvRSmmgChbAOuT3%2BZo1%2FfqkNHvAiRzXeOuzpvDtT5e8b3AbpCKpayFHMCrahdei%2BFH%2FCQbYtaqQbVLhy%2B1EULJP6V9fB4t5UgtRNWkHuMUuHbWze5DZYjfAsqXvxdDntvhM2HAwiaKGyQY6pgFP3aG6H4H6i3t4VHXkXIQVZnr8Ogb0rdK4ogzkrsu6q%2FcyS4%2FtxnLey8iYhBQNTfC8beX5losltAynXWlHuwuUTtYpGyskxd4uiSOyPlUAIvKXw1FavptLaqJ4C%2FM0kkmiyM7mGymISKVkWkhGa%2FAxUwyBqmGUZd3ORwtDQLlrPRZN5zhRg5VEpjpeF8HLYgJpm8hcNT9vqU62uf9DqXNuBMa81ab8&X-Amz-Signature=28315e2f030a6e498012b9ef171a2dc4b06911102faead6357f69454e3821a47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

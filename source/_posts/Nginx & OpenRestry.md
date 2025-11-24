---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q65T7QQK%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T160054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEsRTfupXA2ToHU%2Byl3ezRbExnkrywn%2FU9u2ZmJ3CKeLAiEA9wIcJ5kHBiJdvUXEmtjt3B%2FluK%2BzgByM8D8eWckq0vwq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDHHqeDrkt6V4e0fuASrcA9AE0RaZ4FbcaIc3r%2F3Wpp8rWMd%2Fm8%2BDRYhIgsPz8q%2F8vq%2FtjvYZr0AaTiWfpLZ2EU6o%2BtrTKVso6cgmObJYlYEwn9cL4YxdNbP6R0c%2FcdA8VEHEUyqLQTkoBCOKXH9isd%2B7GnhWcBU7kgh1ipIQ%2FyOdvHGkvjGC8uB0aZgpJb%2FJ0GUeOcddjcucA56Y%2FQivkTByr7JmiBikIl1RHCUlLGVlJ0K0GmsFrs2zs%2F4%2FVechqa3kqQZXE0IHbOKceJGX8iVbVv03QlllIrVuQDjDW8sjO%2FfyAI%2FAHXp44s16EdWTeV4rO8%2Fe1sI0DLPTWvpv1W%2B0Sv6YjLeWFbD53%2FWYEJOhQYQ3J50P86315FvPPDaKu21FWkl1%2FZJzt4%2BMNx7IoBP2MnNX1LZm9ZwjHJaCrwkNhU3i6iu64ekgJGbH0dTZLPbxhuARX6QWe055aKkX6cluUctgvhNXjLQqd6aehAH5F615BWa48SeBP7WOS6yCKywOy3tYVlE6OXLk47h6dtjITqYIKjSSdrUD1f1FJd3of6W0jIxjj1oFl8jEXSgMasphxYVMXpjOpjHIvzZUZgzy%2FsJ3cgO5hsZz09hkb5kXSlbKOsNTdyW4abjxO%2BW6ON732U1FiGbSCgVpMNj6kckGOqUB2D9c5SGdDdF2te3I3ObsGZyN3Qdkk0mjRf0Caz6h32VKIVCnVrmFnIicRUurfFumTyyAN4nUKCvIJYYabN%2BIVf8Q21VW9g%2FObInT89DbPnwv8CL%2BWct9ViBvtLv6OWJMwWyb6FbKS2e99a7U1wVcYu%2Boj1N8Iol8tA6RoFucJTTkbR%2BTNu8q0Fwa3RiHbp%2FZcDpedsCarMIeuPzDq7xWYts3G1Or&X-Amz-Signature=d2d9c31b31ec809939af152719dbb8934b53c600732a5f2707494ea8f8329030&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

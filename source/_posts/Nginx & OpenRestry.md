---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JX4ATI4%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCMD30GMCwfG37JqxVlUhsBF3jjMIvJxfh2WiGcozPphwIgJ1hGzxTVIApZy6Tnb%2BrUFGoyLGzvOzd4ILIDXEYLRKAqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD2yn3Li9KDSZvhMpyrcA42hK6H72MqzQSN1npIONkVXXydflfUxDLINe3me7nXV8Bn%2FU4e%2BrC3yfoc3em%2B9M5uK%2BJV%2FIiHYXLA8CURBO0aSg%2FyRxGQ7ICHZkE6Hk4aHOItdFzy2uY%2Fc7wr4MmKq118fvZ3i40F%2Fo8MPkFT4vbpnaiIiuceJJuRh4hXYiwdxO6J2SLuC6UHE0%2F3kN%2BqrCznkSq3V5br6mS8r%2F5YRtV8oLe%2FUIZDxrSgMG4toD3eh5OZgT%2FNWtK3m6B4r9ZLSJ9pFKH6qduFX7oG%2BANNVDJ32JuzIoyb8kKu3nhN2zWQ6nw2mbwoUb1ItOGyPDtQW5%2FKRjHazdRr0bG6SoB%2BxSg6Of3fAYS3rD3YIBZqnUW5VjMICwbCjM9vryA80rziTqoSu0vUlMPk5fi6Tj7ueADOcZJ1S6TM9lqdUBO%2BWuuQb1bgG7SHMe29p9OzEJhU5Gv4xK18seS02kC1Hj%2B8y6EU6heLjFjCoTM7QNoW7erKHjRBSAX3wNoYW9L%2BQMF96qO5wZcUHIOevt8XS5s94h5ynb07Q8W5FCfVtHJ0HrASid%2BCX%2F4ba99Q87%2FucPdy9McqFsZl%2BqCtX3iwUQyN87A6Em1azu7BpO6DCmpBsL7OtKWangyAlR7xBBwKaMJjj7cgGOqUBjZVRwY40i5Cs%2FyfRB8jT8f2a2HAPIV5nW1iRjlD%2F5FhzV%2F85Tf%2Fzkkf6lG32tuZDo0fC3eBy78mIKTl1riXNqdUKTUP9PFj6Y3dU8VTTe%2FOdtXTpomKm1eWa9Z8q6z2gufeWcauZaA%2FTmn16vBUFptrfAY04TtLWOm5uf9%2FTnN7yOXxhRda%2Ff8PLBuPbn1CYv3c9t%2FPexNEWqwVrl27zIjV0B4XV&X-Amz-Signature=3aaceff042478ea9a05073a473fecaa9826f314e1e0e05fb0f2051c62942df7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

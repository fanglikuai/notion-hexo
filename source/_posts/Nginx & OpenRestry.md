---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3LETS2R%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQCaKtrRKIO8u94OLDhMq0e5ghm4hPP%2Fkwt9Vn75N%2B2qBQIgadaTaoU8%2Fhwl%2BHRmSZRUU8JkniR%2BHctMz%2BY2SJPxJAYq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDBz2pIt0ZeL4DCsWsCrcA%2BqGjcdh4mKeJecaXHhkGmxYI55fA2oA1qk63TJH7rpVXD4rZu%2FZk1tQuGK%2Bcg5sLy%2BeGiQ1dCrNuvQb6Gc8Z7kPqBtsmoJ0xNIiLzvH0Wt4xQZPQywG04vX2yXA0v6zqSlMYfczHpSute4zf7GeFwQzHf%2B3BUXnDV1udpUhLKtG6qsPs3t689a8c9P1xvVFTHd4TLaVI07rX2Ci23Uw%2Fy8g0Bcvem%2BLcDjV0nDAIWZdei1VkkLaoFGKbPLge%2BuC0LL%2FHUbnHFDgxdqQ6VAgpKlr020w8plfFZ0DUz%2F6dvZgwnSIvkZnPyZsp8ttBUAOTgNtQfvnrGZ4BrjEGoe0bvSnlzCvx9rRjg8UqpHImTzObvTgvLWQ8aaeLtKV6s1gi%2FkoefDxsFPdlZ1SHKinJA8hm5yuLwvO8TdDmRuV6qLFCUX1LeZsR0Rxjbmh9ebNX1Dj461Wf3k1N%2F01Ef8A6c8FP7QVzUXdAhXkEr22FDUDBJpb3GksFDtSXdERu7CHBr4i6uvGGq8EZgKskXVeEyN6Jje2azTynEfIM4wBzT%2FLZDTKFev8aSRn%2BUQYGvQZqlNlhmKkbsTRM7WXwfWWZZteCPKo%2BFQUvD8GGWgZzLiQPZoOngtIydHxOvS5MO%2BghskGOqUBkbIWXeVB4SP5CjJActW8HtHKpMuKNElOvsJGoIGHdICDTMY1A6HZVwQ2jU71jm1ex9kmuBk%2BjhfK5d6Lr02LfeQxyGk18CN2rPtYGX3AiEbpFzuKsyaH5rcgVb%2FXqqpJawiXBJZeKO8hCEHRh7Lpz1w07JP68g7Ri2N1dmTwNM7LmJVM6ingPwe2vZoyLdh7e7YddBWWsNj3hjy%2FVnNxhZRV%2B9O9&X-Amz-Signature=091576c19f0f9bf94b6fd2f4ba248af154e65c92a6808857ec42bf93dc71b085&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

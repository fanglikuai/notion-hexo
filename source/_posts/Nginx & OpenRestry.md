---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SSUHLRY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD2AMWoPR%2Fx3r8T5ZDY8qTPnq0I7yZxtID2vpbOwCwWbQIhAIctPpLfVh8m02FtbZeKmvYYXXl35DoU19q0a%2FVTuRi%2BKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz9O5ZqZ9BFw9yVgNMq3AP6muGcLFOQwJ3sP47Xke2xzoA9hCvMITa35hcPzYzjftUYQCoQ1fKoWDmHnOBYaDtAWZeg25p%2Fzz%2B5Tew1c%2F92IW3AsFQlJTY3sZdGGSLhpASIvP6jLjhZJ5od1gwfCBBGLe94mVUIRs%2F18Z5bl4Nhc5YDATOC%2F7QQ5Pbx5TmcWPbk%2Blo5KhEyOi1gL9sLE%2BlwjswPdIozjUmhMQB3VUXBwpbDMa%2B23KP558MYKIYnp20o%2FXUUj3uICqeQdIVPVszfFYdxHz4CNaX%2BAkVABXNjTygvzieTM7QfDhTV2Ngza7%2BCw14Pgiib06Vj2tIgWe5OOQaKaRM3Km32JizPuo2phBDz1DO0emM7VWVMYui6CyxOSwznhhW7nqGPefgetnWW8a3ISmlcr%2BqeO5p0NqiAAJO8ao4SAvVXPC1JCg6%2BmDb2nGv4o0N59QeO2sL7KiqB37PswE08mmmKkfzgbl%2BPN7VQKQtjALD%2BK1%2F83YEf7YC26u3pBQRmtJhKOhEs7GLc4BHWuj6UKFizZB72blSf3JlomiQkWSRSc670%2BsQnDPcFxLlSpLC7R5Kk6kb2A5ZpawKbPemTPALoV4qv50DuQaeqsuffDQ%2Bive%2BaixTxE8FqR2SwcPtX3MAnOTD%2BqJ3JBjqkActYvwaWKcwCVYzl1AJFBs8S1gmCOjCyHyCB5BjAKR6EIH8n4gUu1GjZHmblhRoLdFW98IQUDm1x2PgmKqfMIY1OdmIuGaF6d4zNklBvgVPhpB3cr4wVkSg56hB55OABWwiTipcCsqf06jP8wdYDuGB7jcIgXAurzzs4GxWfGoMcQ4UomxZYRpM7xAgph2C9VotcdyuzedXKpVk%2BpZp5D5iuIZzs&X-Amz-Signature=55e4102edb3f55d695fe932768f0bdfb3cb1cfc8502c7245b8d1c276bb723847&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

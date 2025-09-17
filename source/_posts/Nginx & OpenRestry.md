---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CLSP523%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T000045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJGMEQCIHouFWs7umBK63iXbBqg%2B5KvlSkZGpbfkZ1MmSU9olJZAiB5I0JZsldDEzFfizhYkWr%2Fth%2F6gNwpSKIjiS0sjL9Y%2FSqIBAiY%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMrVcCzYHlsNRILIzuKtwD07GN%2BukEjFY0toM%2BLoZ7xOyKSyV3nvbxaKky%2Be1gDfEs4ZEknitgDhtuA7tE%2F0rzNUP69ctUOQxfWl%2Bkib4qvdga66Cjk5rhGPo4Db3AYgufI3pMkzMwilvrRQUQuewJFe4DG%2B9LRQji23BYt%2FwoNh%2Bogwg61EYH0VkQ9XtgpO8n1oJUJom4%2F84mhflS3vzsdvgf24nhhOKmg%2F50FYxoDPfVPwlSFtsYTF%2FGy7COzB1BUjlpz%2FbPheWTv3wUFnJhWrtAMJXpOBGMzlPdyzr91bodcIbOEY%2F0LcFFh9w1ePFdwJJQSwMmBfxJULp1jOsKUiEllWq%2BRhPSifEwICi%2BbY7yC%2BrYrtWWDTmaXbhBThXvgXscQdJAVVLII0RL0bLpm6jALQli5NSjx2a9d1sY8AsAjU%2BJ6tbB1dA%2FOL7J0l6cGQX81WcRtyoSWkRqgKmJM422aDnrScqP%2BwmHF19a%2FuVFcdyucKqYdANVpDCnYUbb7cOQ5Ibwl8lpJZ4WVvCNfqg8CtsaMQe0cMrg%2F3%2FOJpvcFjN6AHe90DZaAvdGYF7y9lI1qFZGx5zGQwTbNH%2BhIxZjHwWP6ZeTVjKxLuhS6O5Hev39R%2BlV%2FaQYIrLXsMlbuSobV8FjLh7qd8kws9ynxgY6pgEWRBPQy9jUlZI1F2x4yyJD6xR%2FjdcJRFrZat1GUeZ7gr60aMP4NkQFdqKRQYXGpttzJJWkjkamvWF%2FyQD2q2u32hAQJgpLJKgt3wQDY1fyrcF7lQmykvDe%2F3U%2FeieKFvLO9h6JFgL9Wsd2YyoVH0VmOFPG0%2BeWOrrEQ7EDVUmmfAGKhxpgVTWGushPFg59l5yw3UvJe81RWFKy7oMDqMpW0nTa0I49&X-Amz-Signature=19bc19c2e8239e89155f00598648caf9ef90520246e53d3883175797f2ee2933&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

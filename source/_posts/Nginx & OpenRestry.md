---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVVNWANL%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T050045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDn%2BXAJqvbPwWV%2Fmk4skdY66zZAq7QnHFaU8pW4vZXVQwIgXR3fuASADqC7%2FGp4ifUus%2FIT%2FHnQUN1J6noDyuwwyxgq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDJqOAWvKuBcgaRAqtircA9G1OwqMQnL0ERYvpmOubbPmNxfiWNBVK1eXt%2FGrBU98SR66bEjS9OmuoIsdM9QWQRy7N05tz4lieZtg7SeaAq48MjyD6Y8E9ve62o5PymQeu1CBIO5QAE6dPeWQdF0XOmSLnVC1UjvW%2BpO67V4pNk2JA%2BTNv02kICEwjp31bLqCw2vyADT1iJAfCiX2X7B%2FZ6i1Q6xmlnVC%2FUgKOczzsVsWI%2B0tz9opupslnJrrnbbHW68GlSnavG4bKICptfop%2Bn6EJDE3GpbCCKASZUdlQXLTUrs1ZuXChcwDXO194uf32WZAuJHscbBIcn1BnCCSHuPX0%2Fze5WF%2BBAJ5z6XrbWMDzZVXm1HinhVRE31YPPZ6DnEGRDKUSXbA%2BvIyMC5Om%2FA1u4TBVDfouW92GeC3XlYKhZglfGqhTGBjuW4z5LwmjO7kRQCNSvI9JYaEW2gWrEQGABYpiJMCaFKpSrXzolzAe2Ta1YBDmSwtkqj%2FwOMM4%2BQKTzOufA%2FoQQrT4WR2RJ1Sw4xbOwyh3is53BEcaxJFo8mMuuWop0%2BDbP3MIPwaVDYLM6GHREwpVu9itAnqyO80BA0Nkrb6oP6AkVeM4Q4tjW67H3Agcy%2BBCILOMzeo1pcwgqvoa%2Fx%2BzNO%2BMLTqmckGOqUBNfQpDz3D33zOdnuQ6U6UssqLrRBCZW9WI7G%2FW6A0TJogE03LW%2FORvcvzzJ4g%2FuR9XwoZ5F5KdTImzvuTWQ8VIpyVs5kwdqtse2EmO2NIL7l1drsLk7Y8Es%2F8auWrvT4FstSR8txtdq7vyuOmEZZIWtHVyscmaZlK%2F900aDB8BSUzZ6dO5ixJfn79GNa1GkLmvXVEaWC81kqd4y1VrGhWybBgWfTW&X-Amz-Signature=98d9327bd55be07ae80ae9ac9123c22b77139c57c02007917ee7db0a1214eb48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE4U4CMQ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDvQ6ka6yvl105BOnldrbRX88coK0Kr6NJZznoEMrcWcAIhAIuzfPkg1H0QH17F44I4t9G5dW%2F26kyfr5mNJeI6G0xGKv8DCGQQABoMNjM3NDIzMTgzODA1IgwgF9H43GUIkBy4ba4q3AMxQaUlnE2WFM6wTs7HykZYR7W6%2B%2BC7TtiiTB70SjqmESQozqDMZ8kw1JpXAsGayVlO68mtwyVGR6muZciiVvSXOH9ArhbIWhg3dU%2FyXjZvmSZPmsk%2BAmgUHaU9oEsg%2Bs1QC4lRNMQixzISP4XHv2%2BJSRSR%2FSHoF%2BP6gtmDjnwPnkhTqKsKs%2F49c%2FrqFVDnz2WR2sTT6JGvYpDx02Ig2Q%2BHtzMY9hic14NwPYsPhY1T%2BVyeUMzng9S30bp8g%2B%2FBmFHg54uQPNilGYxi9Ti38BOfGVl3e2mhoWUchLj333vgGFzX%2FbqkVSEZM%2F3QrQuUPPSW89%2FL0TqH7jH4deZNKGFWQNmu27QF0mPN3rQnpNaLyVNmAF1sl5W%2FICR6XjwFIgFhBdDm%2BB254LLuw%2FETszXRwoEKTHe0oSYkR2Ieim8VaL%2F%2FnXcWZAQmAU5FPxD16shjG2VqqJbPJKsFicOKRv%2Bq2dJxF9cLJnLNF0en8WPGhgUhG78TyxIGQcNOIC5hf8DHF83T7csBzeOiy5HFRM8fZws1q%2FSR90N5vaOd3jJIe2grYrxGagEkC6BSu1XvWBJwB%2BmfOT7gilCjFZpCqAcecfmvHZWSt3Gepa1HPP%2BnMMANhrskcJkNJaJDLDDt86PIBjqkAXA7vm2OY0ds5OMgcwWsbNgdBIXQinx0IylCvbGc3HHv65OFT4EEXnPohT7c9ubx630zjjk89fjn2%2BLI%2BpcVq7%2FKzHGzfVi66cn3tSd66F2eGErbrsWOiufSFFchSx0x8moAxWoJJH9mONIWkZd%2Bli1NghtjhH9PbsFc1h7oZOjVeqb13s6B2Wsl0A1xToVOzr66YJgEhyG3dX5bNK0eJRbwBuN0&X-Amz-Signature=f66317ebaf9d3a63553f60494fe3b47547e55d153bad6fc90f8f108f037d76e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

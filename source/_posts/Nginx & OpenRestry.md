---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RFSN5QMA%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFLvYiXImkR4fzrMyevS9xuhbzWYE9JQycc0Tew7GVXWAiEAkvuVApatXs16yLhioiC0MlHnR%2FfzeJyxBxXcwMojYicq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDKa6wqZ0AVXt%2BYygrCrcAyKRIrMHV1oOfKyDFIEmsMwgd2tXO8V2vXgbTIMIV5%2FMcIq0%2BA8GouiaQPGfyqy7ZdXqFQS6%2BAaGgLgHtjHmbRmgOIGjUWt%2F%2Bfft8aNl86zRbC8zU4acHXBrkC1XBOHOI%2Bp5S2BCzE%2B8Gw9qHxCiGdoNKeYTiZrrjB%2BpyEntwZuoJ4B%2BmcaCG6Unjy3%2B4O%2FKo9BAgdRj%2F9F5JUJAK7vlsJ1sdAPJ3I2tXw%2F3sgwQ9mWJhRieZqzDGVLADhfP6jtEKithfTIKwYbzGsLY8hBY7f1LuhrXZ0cSn09S7sSz9VvDQfUlT4hTB5vXbvMrBxTJsMPLIsYnecTPuRXEm4IhhJa041eVrXWTq6FpRfpmLLW0pYY2aDdP7Aa5jRL9ruvMYx2OH2WAdHbv%2BoynTIsYS0%2B3HAeLdm4CagrsMVNtO17Yz0KIyE7tgh8CIxEk%2Fjx%2FJ22ZeQ1NqjEes8Ed0rqvSjkR2Lf5iIH9ltEyNgoTT2cOYj1FuCcMFVreF4CkjcrJ6lyiPfrTsCPeRwci0h2Hql36aFB2x7sAwtlX%2BR9c4VsTeyxN7cZR45xKMlKjL%2FCdGzG4U1wzGVf1UX9giVtsvL%2FMQ4WiVAUhtsumn3Q%2B7o7CViVdFWMJwiwepvC0MJa3kskGOqUBdU8ww9ZMyASXf0mVnn%2BO5pUpnmwJhhSBV03QwLeVhxMFGCVwTJbwFsyshTJsiTxT%2FItTaUnSZxFXnh9TQB8B%2FcHerm2XyCtdf%2BwrIQeKgpyFUHfHU30HeEYwKfS%2BHjCc1lJtp%2BIL5iKF0pP%2F6bRhcHA55jGZg2AE2v9wUNusFZO30i7WUnr0mh0Oyu9Salmv9o39dMy7cQckDhkFp7jrVXsOJiG7&X-Amz-Signature=08414f6bdcd1960d4430868e8990fbb4f0350e6d1d1d8687224e9f4496d40fbe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

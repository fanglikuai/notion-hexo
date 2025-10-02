---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJIKXPXA%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T040051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoXnZD2fPApahhX%2B5PkaXMjRHndyEzf5hweNlYtZYbIwIgbBGnv%2F0B3itudylFIg3C5OqV1icpHzoUm6nimi6jExMq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDBoTnOc6t1XvD8QbCCrcA4Oiffi5nYiUfi9cmaMDwycZaydXQA655bJk0BsdpvdCI5XPJrwylK78aERlEUj%2FUb%2BQgkT2rRwgOGJW7vi8fSoru3Ll8Xbv354DcFZrt9OBuyyaGfb8ksBzb7cBq6sDzKeN3J1idGU%2Fz%2B6IoTt3sXsA%2BnThyhfZIN5%2BCBqoGP74YWGbWn9A3NwTq9gJ8ysnDNsH%2BkR9ygBaLsZsB3vhNp39Yomkwoyc2SUgXiMiUTc8%2B2FO%2Fpl%2FUdOzjJ2lOhcwZXIyV6dox6ok%2FmQbTfo3s%2B58Z9ubhNUhScYa5kJEbrhRVoLfNLAT0kzWB5%2BiUNm5FpU83lJGa8H%2BTuwtxahz%2FkP7h6NlBT%2BkfvwPyweIGhwGmLhxDWq7RCIJLlbjjoeaxpqhjqsJ0koAFBht9D6ghizjUl%2BN8czcympSKmpLNk6vwaAS4s2%2FsjqBJR9qCEYoRC48ufCjlnSAp7ndIFdqTm6aV3CX3DFurgQQTUnf1TGFa3LkqEkFbz59yZOi4woCa0FtW0pNBJttmVPXRtk0n4vTGE9I5QtfsggAXGQZnEDlAeayrMJerimL%2FG%2BbFvNXwJUNPUBxSJ1mpUri4afGH%2B8g9gM4bKzyTjfgCWSa5UPs%2F%2FQsGStOK1JpNjEmMNDe98YGOqUBbGEqOjqStAy3ceV36i%2Fo7GZWYc0LJ3FxMS4ARxW8UtJR5pjWJ5INCYmjnSGZo%2BAxzbhedfpjan12UyrzYPSRdgxmKGhgMQWbH%2BGvxj8SDEJ1Mbu5KzMumBeydT6Lp3stX%2FxOfb4zsfdKsqduC5C3TJsZ10QSqs%2FFxORrzGe%2FQXiPcLWNQ9peTlTWtRlO%2FAG5WuyN95%2FiP6VF71OOanIYM8xW36UT&X-Amz-Signature=1738d474e1fff3aadac936632b0f1f42354d69a56fc11c7cee529d0b4095ada4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

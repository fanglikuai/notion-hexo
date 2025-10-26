---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XAVU43BX%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T050055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU9M%2Fyt9wJ%2BkRGyD4wphAIICg8Rh0XTyjPzGjpu1YlPAiEAsP%2B9Cece%2FXcqWQQ4S7ycCu63GIA0C%2FoqPIBBkGYbSn0qiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGEWiH6lg5FjMpGpfSrcAzuDhYCTQIf9kxeespYdk4gM%2BOqVoaSrSNb6RVn8p1kWz81VZ2YaDkv2lDLWDLjjfBRc93t%2FvjFIi%2B%2BMtOEVewazfJ%2BFUudRZ6Z56TQfmJzOKWdswomJrYsdcBMwN3wkT50IPqV%2FX5asdQhtobbdNG8aqO0JXbqRw41AbQWcntuxTJcQXf5dKrM270mH4piyqafhIVd6Gex7PzwSy8Zqt8QVCu2HUTgh750Rd9ucKzj9%2BkFwGtoi%2BFr5fwU7schoTsqtI2vGgI6h6KqAi5lCrJibtrr6zcJ9CmYSi%2Bjomf8X2xlA5%2Fo6hUGFgpg4%2Bpqu6MjRMdUGorjGK%2BK45u6D6gvC%2FTmFeueHVnKmppwDHzXtI4SQSdEUGxg6cr%2FEdtjUszjyW0P5bQxrKBtorkuTpq%2Fz0AwkKfBbE%2B8MoqDmF6DuQU8LO8zXAYEoA9QkV31At1tTOwo0wnnqQxXXT0zOhb8PunxXk9816PS7w6cPaoUHWD5Pf3bzamxdrTBqSWgVtDAy1zEjV6iORZc2PQpRYZRDIUhmkucno7RhY0serzn4wbb5akjLeiDf9c4qZQzbWgcqtLnk6PjKf09sdDQeZynvTDWwr6Xj9M4z8HR1AT%2FL0SkBMq0mmhCpDvAkMKLw9ccGOqUBdY%2FLjSFPoRuqObo0N7M9KgeMfi1uBpmB1sm0vshJ32zXaFrOKhZXQ10IWuTh1SXhiL9pU1r%2FJPB7WTrIBEblKz90EU4jk%2BufEwIcKPz3GZuzpgzT4T1ksBwL0se9twwAAc4irucltiqwYdYVAHCCEOtLix8dpp7Hz0lhfUXbMGe19n5AsFG%2F3a%2BZzrf4wSYYPu3Y05DmkXoFeENnVaS7Z%2FINYA3y&X-Amz-Signature=79a195891cfb69ae5ac1a6bcdaaa630828a2d648c324eac963df8a6bd9480d1a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

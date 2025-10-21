---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4TZBD6O%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T100107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJGMEQCIEm7sFcGrzcMmYI5yS9gfIbAp8c3ElAA%2BgRWtEy5DhJLAiAidq42cHjUi7PJCDxb57fe5pI%2FW9p9YrToMdgiJXHPHCr%2FAwgSEAAaDDYzNzQyMzE4MzgwNSIMJDy4oybPzFDRgqoMKtwD1Oln34UqBuZeCQ3OPFDKkyJHOrfVKMalhze8ixoJEX5v2tL1M3S1eYW04XNBjHvRkehWIThX2mHFIqi7lX9Adb3KmItoCcuodWS%2FPyOGxIqftA%2FtPC7k6FtI%2FeyyIKRJ2M1Y4Dh4szxR4ODK4b%2FGrpME7iYPG9slvYYz5jPYcC01WV0lA4E6N2O98%2FUdOoWBlG5JyjlRywUWJROpIxL2qyNVc0zUS7QDpGQUpjFaJtwrYecA0M3r%2FkKwaF1u%2Fln5wHogNU2Vb6OwHJVTEwNnMMso8KvBaRg82YsyMit11g9hZ2C25h%2FFObJjsyrHQyEztTOMcXtoUakGTjWZCWSvx7fIN4fy4Oh5pWeGZiNnVI1QmH3zFe6cNRZqs4DCbOYLrNtghvBauKSbli3JDJd1bAucqLgwrZetI6nI3l8WBSBXHaF%2B5P5FpjptnLMRKieBMkpwD8EPytXLL4%2FFWoNy28Py7XCWQsHV2P4FLCletC%2BCFSuoZa2tvKf9UdPIGdamwiDBrRtJjg4Uxh1kRKZbljt9f1Gok%2FPdokFxZ0c5hq5Ifg80lGW7OXsNxQBNQKSx7HllOMxmui3eYjCxWBdcyT%2BHBDbIIdcfLVOe6smP%2F9BZeCGiGr8Oe3fmDCgwgZPdxwY6pgGDh7YQqlOs8oaOMoz9KE4ods8BgwknuK24HMh0TL66HIGSZLaCNh%2BwZ3tQkrEeHxqCnIvDGrgM8wT0sJ%2B50JZ9weWBVh9JfQC5IKe2KxWwWB9YQuSBaQpBpwfZ59Rl0n%2F44or6jzm77MwOTXfB1OCXGwudJqn%2FvrJctqMfpHMbJQ9oPzQaOmbpeIxhVLx1toMdvgBt7w3%2FiCmRJEIm4eqskCV%2BiueY&X-Amz-Signature=e6935f1983926c2dbc2ad79bd5ec601cd17fff793327cec8f3ec4b590a2ef957&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q4Z3PKOX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQDHCvXRL5Wayyue5cspdtu%2FlKeaF2hjKkFxSwJKzIYkhwIhAOx%2BKKzhEqOGwqhah2Vdp8mzRAB16cQn%2BXkO1SPJ%2FEhXKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgygL9mc6UjCezj9WZIq3AMFhzhF2McEBO20xJgwRqRgytMM0DrdJzvbaO2TJdxtlbXngIlajafonunhUbQ71pdu6zLRJY6vDItDHAl24aAdapqZ0mUZt1DHgiI6ZuE5N4ziOLhSZcGQAxjIwkfN9%2FYzTc2orqAemHR%2Fe7EGxOFySQrZ8lKjPHoWj13MOQYdPah%2Br0fUgMLdou%2FHotSwejCnYryLxT%2FKZVGRIUBcoq5jrLRC99F33vfCEw%2F4WEAJhXicMMWW4hjzRkFZUSyrdNJluqLf3630%2Bg3Mf8cm7kRqrzcxBEUgdCLPwack1lBKUwkEjAPQUdfVUeyl0yLo%2Fxs7Fn0zo1jISupwMdS4MB9l5aHJenMW5wNV%2BLPjcg9Ag%2FljkGekVBpm53bYeaR8F%2FigKP2fviEoZqQ89yV3cXuUbHTZ%2Bju0%2BELg4AQ88o%2BD7PokbKSMdpkuDk8aJLLhrOf4sukmQRM8sN05fsPyYwScH%2FMo3mRPJLZrG%2B8UHnGedYJVkg%2Bqv3D6nL2KBSaoxmkJqhchrJW5%2B8PEJRSJEqolBjkLu56cht3g%2F6ksT0rdeaIMNqbF%2BQNavFNFKl5jqrZrEDwLG07rHcsKg2Qow0gJDmy0J%2BsJAJjuxbeF9HKWD41OTmfOrkkOjMMEBjD1yKPHBjqkASL4xhHFyYrt2PDJ0m06x2ej79OFpundt4vH1DK1WA6Ddf1hYrhUQpAjOOt6D1t7AXrNgdJRgMxycYbdS%2FlKJr6CN4jjW%2BWYM7SJ%2F9Z1r9r%2BIQ3LKiR6i05HaC2Kku5%2B0cycYNJ%2B4dg5YBGXx3raOsVGm8B6faypWWwFuPN1w5NzxUISBn%2FWW9%2BgGjJU8rgm9SuMsyjns97%2BbqU4KbLZTbdwuFLY&X-Amz-Signature=2bafc040a3ebea0a0aa02331377a6cd692fcb2e58c731f67dee299a61da14142&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

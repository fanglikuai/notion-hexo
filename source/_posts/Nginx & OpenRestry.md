---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRIRIOK2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T140058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlRayQNWihGHJkelA%2Fu6tSDNFhTsakkzY7faCZOIwd6AiEAgfGqIMilknSq35Fl7XjXecHg1t1wxqY4hawZ6AYUDNkqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNuauyf2y9UCNAWPeyrcAwSS6mj4RB9dkox6jmybUb5VgWmZmlrucuPFcwwkvK3HF1jxShNSQqmesHFInuQ0b0o0KW5P0w68LU708l8uIvapqMRWOiXLyA4f5F5NPTwdVqg6YvwQYoEi5NzVvqgWiWQXcSQiOIjawOvFY%2BkllcDB0qsZUxUHRF%2Fs2ccrdiOSEZu1qoYdoNTxpxQXeq9C7n1Shv5HFFJ6K%2Fj4abrZpO19sggAahXWQ21uQ5pdbBV%2F6yOyuQU7eYFSmWooYPhyyJWOXZukFqv90Wyt%2Bg5PaopJpiSKmQ14WtmJMw%2BM75SE3fr9SAdYYn7CMGo54JQh8HFUf7sr6DQDIz%2FcxWdz3TrRZzPdususqT2afQ82LwHaJZKnEFwRs2UDQZRoOiCXTUqs%2B8TMYNQy45OAeEHx4ThRlXs4t496kSdZWn3uvMNcKO18s%2FeIf52vYNmYFiENCy6g5iM05bqTANWZV%2F2h8GSvaA10gjKdofjt766teHnnezJlILvpjXDIVKbomz0dqyyX5oJaxwwPk1%2BeW5k0O0abIKGsf%2BKK1n5g6GUtikLfRvhb3BIvGYrxrlzwNtLiFaTHnWRQ3suQ%2Faxk3wGfN%2Bd8%2F9b4pnEs%2B%2BTMeOwWWkG0Lb7BiDykPLFSUtPvMOqTrcgGOqUB4ZQkytYTLEiPtE7SarXUti%2BFFj8Ti8YbN0Bsbzie201h1sFgEuJgTIGQsaZr49GMW6ZOkyOO39lGhs0bmKf59UakryghW3YcKHKBaW078QeiD3%2FBUidfFaEwWk6IV5kXlQS%2BwCGrbjFRdzXvCd9JCkx5bgPMNrQ3VchxrgRkQSRpoac1K5NnXRJVuZabSMPNreo3XwrKJezO078sGXGvZ%2BK%2FBZhG&X-Amz-Signature=4ce4c76d42ebd467067f84d2bfdc40a3adabefd69c99f5684219d8b389767127&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDT4XG3F%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T230044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQDb6dgQbTSqGRVYtdUmU6m79uakrA689XuBAz%2B%2F1%2FwJjQIhAMkCeLHkI12uFfZ8eA0LPSG8sr5LRrrb2F00J3Ssx6WCKogECN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxlofEgfblPloQcLhwq3ANGFP9DiSBk8YJvB9%2F29jjtKQFYJ7AixqhUoJP2O%2F715No5pgXRHTOhrnrRLsaraCSFPI47paaC3KszTn6JSagO3ivRHpUB8pTL4QVJtprFCjuu6zDCbDoS0e1KnF6TYIWbD6hS8Gqg9odkr7t8Uc0AxXkOmI4pp8%2FSftMIHfTpJkkt5NXbwvSQhGii9tMwlyv6lE7ttTqRfeOaPmdIWCAiyWjAbJqZrTYUHDA6doSmEk16GqXeQOWKtrCP45qqx0A5c2ArMKPnOZURWEHgIc%2F%2FcNK15aBoZwIeuv4srYJC35qJuXc0e2jSs402F2572cUuPg5B8qmvyE8aJKLAlZbXMPhEoUhIx6aG6CiwxJzsDE2hviAh4rt2tz1vt1zim%2FdUSmQbf%2Bx3fHQTbmsXpK8qcZrm4FndsYU4DPK7E%2BVZ0ak%2FarwbEaHmSgmipHkVTLAJF0NJYWkWmsUezmWr5mn8EryMJ35dBA04y0Kx4GCKd1GDBFO2f%2FSdDaff4WTAGXKpisVKaqCSBRzKZIlB17AI5gzYkjBWSDtdJo2Wcj3B9EVtp0LUG9VEYwO5US%2BPW4h4NfwM%2BHUufwtI9QWUbqRE%2BiWrg%2Buw572QzeeS73La7aHHY17R6znDLPk5TjDkiezGBjqkAY3AeXvf2YlsJOzFUeyqBL%2BtjbT9fcETNmrK9A8m%2Fv0VE68SYAAMauW9pGWwtHLFZUrA4225HmuoacA%2F5d8%2FdGFa%2FzXzUKPD%2FcZOq9HAsfZcDCZwDZgLETzfz1hVf98oczNc8YKmWvlBmvfXQOlDbqV2oJSGHAvsH%2BP0vsgpAYJ9kT7B9S9h3otGcuTCXkugQHO1UMFoi3ro%2BoiKZvaOxOf4uAPh&X-Amz-Signature=dd4626481bbc43939cf211c3378fd467ac9a82799361f0206787fd158fa61c18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KCKVFZ2%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T190049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXN909XXCdUfvMSFzhQgIIntae6BO%2BYtQUHzzzDcRiNQIgU%2B4wM3q768qNKyuGk8m3KdG6zNCmnUsW%2F1idysiC1v8qiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDr%2FrqdR2r%2F1QFoyPCrcA48faWxo3CShfg73cI1F4J%2FBLhKJHZCYgFKFp4awkFXLkwS7k8X1NFFpbcrDVOXuq%2BF4e1bRlMyulzP%2Fs4tWvNhh78ya1hSbYW9Im9iEjGrX6PDje3%2Fez%2BwMpoGYAlk4of3Kh5sye%2FrTwM6V0rcPSh2YnsC25XuZbRfzV23mUugI7IbKfnQnNrpXQ2XNA0%2FujdI1JwOsXvw7RX%2B%2Fh3Vx3swTluwSoJ1S9KWGp7kK55lg7w4iU8u%2BgRgtATWh9RCyqzmj0%2FHFcPUWRrkRHJ1gCJz1xBM8MAnIaSf5Fi53wpr6H2NPYddvXgBDrhm4Edc%2FiBOWQIDSlu883BAYrRlIUVDqF8OvwTd7rf9wpZvlnC1QUxnGoFkQGsb%2FOUwtYSCkiHTnFuiw4DWMkH2vWl04JLcH6JoAdxG9R9VePgEw0hcAdV0QXDsGvs9ZxzQtxu0WOAkvLgtPpfZKPJN99DtQ57wwv8SYP2KGvPz0Zc97ISgfL6rZP6r2FonU7DIECJYj9Ht2cGvcj0GzyiCcAjHceIYtXGSWo6OH%2BxAc2eb%2BgDgJnQeF6eNOFNBHC0RdlnzOqJZchdyyiR374oWrpCXGrtYsaZJS%2FInaUWdynuQDs%2BU2LhCfvU5b11Kg4fw4MMyh4sgGOqUBJ95qP3bsMShWDRot0Xyurj0WVc4eNEsFarS5h6bfgBtyJEuavlvXATFfjlEi9s0CTOJAZwawMrf7xD7kOQTWnLE6QPo8EeqXwsI7qYr0vH4xgkf5Zdo2FAn7o0VfkKQ4fSNnrWQxKdyERJ96KigS7jEI5irhCM8IkDuRJKe0tbomrEffbOs57nNaS26gCrVrMsn6CHaxXXTDk%2BWakAjQwZGExcXG&X-Amz-Signature=a1a566ae8c1dc5a84ec8b75421924a4511b5ca96b4ceb445c613e8e1b9347d24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

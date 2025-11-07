---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDHKICWB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICZ3yudo29IHhijihO4z0wsD0RUPAMjDBSaMGP5%2B0SdtAiAt30YyjgZ%2BY5siewpEdeiKzf2y4XI8MN%2F%2B5lTvYMDGjSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUw1%2FnclBC68%2BCwenKtwDVnFIR694SU39NCDdd9yRDPwuS709nC9RcThO2%2F4ncr5vUGEiVk%2B9DZQ4FzsOUZUzyCgTqW8iAxjm9ngcQXz%2BA1jpcOe3qh9x%2BcOTONeLBAuu1ngExl0%2BCfw1EnJ4IJ8pUhiUfNOMVgGsgelfIg4acpjoQgugKs7tvFkUF83UGsrdSRDGxUDtNjFfW%2BZiKcZ60RzfJc%2BanXlbYDVIe64FScP6ddxySwwBCmtozSRwQnzZ56GH0R5ivsRZVvPYaC%2FZI0PCjZHcXrWB%2Bp4zANkPzh%2Ff7F6x8p7FJbhyfIyT5vwlEr%2BjF9jLKtnn42uz87DW5rVfQvdbD%2FKuDO5%2FuRa7coH7mB2pToDsSB9WJuo2YjV%2FnWGVjAQE24DC9OTr8mxO7vXwD1Xia68EV9YXkJiIe4hx%2FNcXKZByxCOCPfaIQHJDzniTybBhZkvI42%2B6yWCk58yTdPdGBqAvWYmFCQCoUZxMlgYtTk1V15ZmV7xIS9UOeK%2FS2jglttQ6OCblxlT3G8T%2Ft%2F8y7%2BoPJ4R0WjvhAfrwjSDC%2B4RAReW13ZJGiOuAO%2Fc1rdvtumrTsVfgvBmKYdznRZ0poQ%2FTswRWrRC9E4d5KrWxcQ4EoMFVGJKRGjoF0yth9EuJ4gyCUeUwm6K1yAY6pgHnDYljwctp6OHA99Oo56QqCz8khAvnSnZKdmt0MBtefKKnfflq2cfUxcVGI0Rx%2BUf6fsbQ83%2BrjFbXtghceumnv5312DkpyOQLcEMxXgWI43f5mUxRtL45FrkUxHnIxgb5X6Uv4ctQ8mRxnA2W3JRaWTuye2zCuVfzw483ePkceCDdyhsV82RIRqIRSUDa8vCSg9ljUnV4z2xymSEyjScQ7DxpZt0B&X-Amz-Signature=1dd1a0489fc566dcc0455f5da9f4fca41af18e65b09a9048a699432ab3eb3dfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

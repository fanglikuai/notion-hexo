---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466356YCOEN%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T070041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCRv3p9xS9vK8rhsc%2FHF0wIypaot5b2qNJ%2FEKIgCIxL2gIhALC4ZFu3RGEuCZGEc9M%2BznxrqjIaYwySrxOx%2BHtP3e5JKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyRl1bUOjWydTJSAtMq3ANo4xlhZXO4iSlde84fHM9%2B5ZeUk1uESB3YukFcPYpJMgmLmpK2GCNTRSApK3zGp%2FD322va41MfrcrLY9vXWr9QvR3SnYQz1dZKHKe272ZAFd9F%2FDAfnBARhc3K08qQXygZ1AzlzUQ7L%2FjFMP564XILb9FsLooFpboWtbCrWcG5j4JpVweFNgi8nrvHvjMESNFk4GdHNZsnrZxNGug9fbvgrF4kX0%2BDGlsHTnc0Lecfe2aTr6lpNNJKxPcLfwb9ke5DgH8Mkiw6%2B%2B6KG92%2FGHsOwUKxvZsSGneql61j6nmxUEXzxdQO86v%2BUC3R7XlGcjlD%2BMs5h6%2BZickzD5trGyJxSHHwiDrrhrbGg%2FsnYLtpRzd5q36%2Fq1DkaNIyCvAV9MatHV6H6rbIFPpQHCvQcG7WTId5dQcJMj3kOYgYfgyuw70AtJdJax4WZbnPIVMdpWKQFBduSRVLkUzKknu3Fd5mCMAj8exd0LRZmoue%2BLCxD%2BsqyAHdnIGeqmHx1LIfTvqQnSXHADYDpBuAzORgJIZVVT6T02N7Zd8f1PDTe8cjLRb8loY1M0T04gfgrIG2vQb0%2Fdh5eeITH2uAg9IZxv4OLecVI8CjFl6FuhVnsz28%2B2JkjOoLRqt0t6KmsjDB2djGBjqkAT0zISnIJofSztL0W3qsH1PpUHMpRVqs8KC8cSTLJjxxpnN1oFUVqL2HDtWKcaZXZDOdA%2Fc8UJyPkPbvP0cviTP6yC1Stoftxeq1H%2B9JhPJdhJCB2v%2FdC5PlM8k5QpPDR0Kg%2FKHI7cV%2FD3x6HTh4lw5KP1TZorDpwgHNvyv0PQui5FH%2BT99KJwThVHgq817ZZW21gg%2BdB%2FNLb8Ho7Hl926tMjK65&X-Amz-Signature=d3e00e368aa2b6b8d1497804bd7c9c8c29f7b6f52023e7f715286c08de153f19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

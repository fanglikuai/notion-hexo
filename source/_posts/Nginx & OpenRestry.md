---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662AHOA4ST%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T100048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGgZB4sJMz4a4W9rZPYIpzQONwTaOyLhov2ORC%2B%2FtL9HAiBUGY8Rp6ml1MOP0mFWQ%2FjnDVWG3V4ASU2QVdC6QWTFpyqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2F5zr%2BqGSW7nbCzb8KtwD01jvyvFAiag7O9jNt3W%2FWeJ5x6QpEOU2IZ4om8o0uvysuYSswJfQhEyVs%2FHcCbkIDCbNWaP8R7Cy1DaWILFIvx2DCSX6%2B2ReJqsistPDpeYIndUyDthXglsw1n71S5RGAtZIZG8KXN3x5qh0oyjDeebH1LxrxfvHHVYkPfONLHrzprxft7YVhktGrebOv2wzJq3giBC2Br9wYn8RyL7sbKLHoPwMubga8TLtwMNnE0jqi0cUMGQuHW5N1yy3t67zeBbroCH%2BuiwUzxS9raTNS2uNgxE5%2BLUGI6uOHz7yvJipAyM6bL2sZ1KzILb7Ehqj9o8OsdUz5NnkyNPWlHlLHixFBJWhPX2%2Fq8Y5wGQdW9ruBvGhXmhxW4BuSpgoIqTFe4AzMM4QvD%2B01oi0h0bY3nM6nEDe3zqXBSGuwI1drqkRif2Dh2luQ6UwtM%2F%2FMKugl9geOXhIOJGztHlpcZCMRd998%2F1NlTqhn3D7fkLZq63%2BhLgy4a69BdPpQOg0JbySia2M79rHJf1KpyfBVYusYoJVRjUauy%2BzCuJCRmhKw1b2eC8b74MO5BJi6JD2bsv4lm5ToAtqt6ZDOHPFhArsu1LzKAkipVYog4lYWS2fL5Jqyjher7BRQu9X4GswnObHxwY6pgGCR1BZiP0FqGxEE5Ezim6Wq7sAGSMFMXR1XhDIbNj0FJa9iBtC4ast%2F6S4E0UF2nMNy4S3sk3s1fGr5drZfVy9yG93MS8PmLQ77MQyzcMDLDMsxmdw2Cw0Mfrk5RgkkiXDDxQqCMRVnDl2bbjBo7gv7STwr%2BTRggK9pNKLATar56BeG2e%2BDxP%2BmfMkPkTaZ5K1cAXaWCqjIw6ZgqvwscgBMFtW7Tsq&X-Amz-Signature=9214314ea842dce7697e5f85e0a1cedbdb8c5332c9255eb231d7f57d27788314&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

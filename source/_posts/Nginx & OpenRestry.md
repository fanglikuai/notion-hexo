---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U763VWBO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T150048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFL3BlfR91gZrkq7ldPxKlZ5N68grefRQ1U8KvtrSlwyAiEAjpLdrtLdAgMjn5ZOGlQnTk28x2UhQimZGZCetZGuAGMqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI9z5XXu2DhhS3mS%2FircA%2BzvZCWOYkrhzUhA27%2BacHH04aSRVfqpsxbIkpLa8Y8BO1iGwzcAz4Dwo9DNUxQRKjGecrUvr7YbrwBbEI57Va8Uw21ETOw76C3BF3d9%2BR2TtfCfu1ch12GpDR1ZZbUzgqM%2BWrqOzpX5cGoqJRV6FsT%2BXZun2uMbME2NPa%2BZ0xl47hyarb8DK7FmTVkslkBmUE8lAYMUH9ElG%2Biss9UAUjF2NH%2FDDrxFH%2BBwqNLQh%2Bv9pvhj%2Fmn204d2bGjSH0%2FpDXK2MrFUH3viAKDcWRwuIffj9GKhBeCJB%2FJiwcqAxyG3ZNc6uWpcioryH1HsFvIQcAqEWWWwdHl0yz5esbsU77cYDMiD8nWe%2Fa9WvMqLUxP%2Fs9sR5nxXrJ3T7uKqgAtsWVzVXQ8d7AuMuf3OfjQNXf2E%2B3uELZAT7XaaKCIlT5NQf3fyIWsLC8MSjZg8kucjLB38G57xmcChVQlW76ti5pG4OPi4UGWO8yVnc2eBAFeoaxRjuPTBfuAhFCLJDDwSnRMiWhsTCR1n4WcH9X2ehtF9SrOkfi3Fqo7%2FMEtDJER%2FKmsZTacg12VUcLKBLsQYBuQu7iEZ5j%2FnURaFyOuWgsEf4ao3wnlHSvTb9QD%2BWZgAPf7scwzB2YdBpSheMIyonMkGOqUBmvSSjSNENQDW8RTqulRIo9Cdud%2BiEeH%2BnLQPY3iTrujz%2FJAaZrAAYGgGL06%2FPJIRGzideGHtk61NwfdMc1bJ58owJalCiz77wKxxc4PbJNewRwDsHfhOn9WHfOjK5my%2Bxn6i5YD1HdxM25Dl%2Bv6M%2F35e7wf3KXKlSWpOsVayCIWhN0nMqVyqHEu3kFqQxSa003jF9WOYGN%2BOvFs7wQB%2FA84cxDtE&X-Amz-Signature=5ef595598234474cd83a6affec1b105e919ae649aa37b6f8bf2d3322a911756a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

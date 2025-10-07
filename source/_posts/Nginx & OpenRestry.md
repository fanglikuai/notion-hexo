---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYBGGOM6%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIBA7VbgTvAaWupna%2Bj5HPBvW5JzVftOpuXowVg7r0iY5AiAxrjW8RlLn0PfFgU%2B3Gc9kWckkZzK7DQYtEJ70COQ2MiqIBAiq%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMlYZUNLLxwWGO%2BjtUKtwDUiO4YH3Z7ArgHtqZlNRP8UN6YycMMGXJBW9VQdsUkGYVRlPAFk4tyGnn%2BzyaUjrBg9JAZhAmTWdWPiGodJ8IkpCoV4EeLqu5So%2BmZYj7NXBCCSJayAjEScTCrUZW8oTIxiaAq19aoASQqJDIljuISGoq%2B6H1%2BTb6kvaRiL5KOh%2BJpAh6b9eZDljIbcSAAfm6P%2B84lAOulrmN6%2Bl%2BjuIlfuJhGg%2BfSoKr%2BgRdrrAiXUr8BzHuIwrXz%2FXqlg1%2BJvht%2FJ5PMT1f36QDBZQePOF4jYOraQRXtD38nWI11eWXhDFmEWgRsWfssYDvM3aOwGC53LuofgZucSbZq2YuymgbGnhikJyMYqOsIt%2FaU9nGJZOq97rbXQshJtsPvVY6g%2B1Qn4FYkk9e47V3ZmZvB%2FYA%2FljaUsbeAPsvreMg%2FkoGgxsDX3NnT8FRD1a1bp30raApN0NThZpo23NdChQT%2F%2F%2F%2Fj0jG1Ugb47wbkqG%2BUsXPoDHiDOAJDdPOBzlRVXpa3eXQdbJm05SYck4cxegIyHSyNjrOc6%2FWmfBK9jTVQqc%2B2qRJSUpX2jOWCexNrXeZ9NiaWS1iEWD0%2FzcKN1zFEfooyTxDQwzEl98TSwutiDibSskdtJmSQOW0%2F9jXoD4wqYCVxwY6pgHlmbNWcfwsBYs%2B34H8pL4elXgWLri87i0faOFgoIJC5nUzesgPWuHooNz%2Bvw2EsRk%2FtMemDbySamOWEx4fAD2miTim7UgLZ05syQBmGI5%2BCU%2B6ac06kUC7sOT%2FvtMjKkx3RVfn4Y0pXPwZP%2FI0mYUaAaqwjjcGAaaakgE5Z5Z9jDAv7RH546TM6i27foyQjl8FtUhyofmHvkBxY1A5GRrsP3HT9uZW&X-Amz-Signature=fa2b55a0b7dc29677de6e438ec0ed6c964c0736e599ac6891a48e09ffb655aad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

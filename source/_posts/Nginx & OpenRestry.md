---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHZEUXMZ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5HiomxDeb1fMr6eKGyQQrZIsfm64zVy59rayqGt%2BolQIhANPOjWLMCxooSLwfOcfthQO5lHN7rO0Rq88spQdQGapQKv8DCGQQABoMNjM3NDIzMTgzODA1IgzaLX6YXYHmvpNr5gwq3APFsCBUvA1ln2usHbmSYQktzDqqQbsBAlb0jl%2FF1mCgVG9uJQ2VZVkvwjRsoy%2FKiCkuVchXBhXwIOHV%2FDKqgdKlQVbPN02EQRVf2DCclKeQ9nkI5i6fgh5JoKBHYx5OQkNPtrmSTNm%2FAtBAX9fDbMKx8l6D6s%2F7bYwR1KdQAP4mK2M6viuWn3xMDHqfiFaIiCJdE8tkXFOAD8lT5RxiFe7K6%2BpNtESt2RsCdEW6YdhdTWUK6HnoL19lxGpKiWPI8nktVKO7qQqOy1XK1EAMdmIj3qwwXQFnhJT8wJ4civVKDfWfJx%2Ft3UPqfX4JmQtwymgX0uuqFEGm4NqYQLD6KFxKXYMdxK7%2F3t2zewTfJmJ3S3rvqpY7SdbAOsJoLvuDwnqQMFmAroAif6nK3baGwYB7FNSRWErKsCUnbxt2UGsCBJWG82U1JP6pbBBTEdt2wDUrPHsJNj%2F5eNaf33TdJasUkeg6MhM%2B9OE8eKhVgxs1KydQXg5bLFco2uk5mjT6sE6tjddfKM9aQyUmPo6zXwvI3cb9f8aS8cjIKnqxTzhbeAm60oLX8E2jugaAhpUzSXXg2TvJLcBew2lzMJtHPct66cFFwAkUAS02BDZ6A5GN4tGJw7VyENflkSRvCjCq86PIBjqkAVAeoAuDLVycwWp0ZzX9Vulv7lpLeBEQEshT5Lxs39dm6SuJNaPIH3d55HV2dfuwx%2BlZ0KbgTEcKYEybpQvTXeeLXQrAKPPSVoO7WhrR4JILyGkVpywZNfTibhTdWgC%2F5UjYsg%2FhSpRkjFxfqZgOn3V8LgrW4L3k6stlZ6c7%2Bi9MnaxwB0HnsSypjLcUaAv96F4ft3qTqlVXbC1389QNZ7D1FODO&X-Amz-Signature=5d7b23e26ed717db3f6741a4cb260d2f66fcffdfbc9c85c1dd20850c12da8481&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

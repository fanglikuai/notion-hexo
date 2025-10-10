---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634PEFAKO%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T220540Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJIMEYCIQCWKYU7%2FDT%2FFb4eqyPBzJm8D2MTEkJTizeFCCd%2FklSyMwIhAO0%2B7nlKDYXEpsNJvxG7Mm%2Bq%2FG4fApsRSpJiwfvWA5MSKogECPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwCakJmqU6Nq3grs0kq3ANI8ffHWDBC7eLUzZi7ChcJCUccbkKsEIBMJBYet6Qjw2F6dXAOMJAoLHJ1owOUiuCJaYGXzXaVq708vWXqzNCd8qLqyrsll8Hpz08dqTUJ03g1RdPBOXSYO6YT1OWIYCgRnSgpJpNJptek7daW7Xx4LLAqN%2FXLHQOIgKrPy7IvZsjpn%2BMm15%2BSPVgWsMVORusqMxJxZiR0roQOez%2Fe1os7zIWxFIlzp95TchPQjnMessYl%2B%2FtO%2FkPRrMdFqt6IJyiH%2FshxjmLyMoJumZ2UspbbzR7j37hcEVf2wBzsvSngYG5gNgRJ3hP0eCwgwTT0S6JTTRRxGdZdH34UIcP5vjCW%2FBRdBBM72fkZmF9%2F4zKpN6zPIeGTuEmKiNp0TP5vEO4WqZEVP2i9SA7SOgQQc534dhFtjEL4sAquBBkfks76oihHuGZIVDL%2F6Ckc19juVyb37%2BypegNZ%2B135D5PSiAauaJuMrHRQWR%2Bd5d5awVp9UUe4R36%2BXFrgXm3aPJEY4FsnFZWlrrAXi51Nivh9NRsuMRBlkfuH01hQO%2BtnTgs3QUlpmetoTHaz5LAIfNpnsOxruv5ekWzBQojESSRrzLEXr92gm5VRPjwTmN5JpQSKdtrpLY3uBhQYgcvNkjCOgabHBjqkAWSaS06L8Z7xphPYwyw3UXzvbQ%2BgY%2ByInwhzJbFruCXII4HsCTQGh8h8G2tRtqKTg4wc9ONv25I3bvoeU1rCKmPnnv4AT54ATw0%2FbMdqCl2Z%2F%2BQJEwdnbSSxKKUgftUYcs4ALg870TvcvZXRvd2NdfWPeoPyVLpKE9HPeR%2FWoGQPtV%2FShhVEYL8pljnrYrrbPvUX%2F0B4SNtWfQsYBPf5n4QMhm72&X-Amz-Signature=d8c8f1a946a69514022a8c63f6b48e37d5bf97fdeee9bf60547500c6aedae2ea&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

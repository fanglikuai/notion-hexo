---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667353DXDL%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCVH2pmgatYQKA8Ts6EGMaY8XrvbSpz3NPIBTTaHt%2BLaAIhAMbyCKeKcU8vKLQKYZgUZazoNyeGEUK46Rp9Hg366ulUKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzpgE5osWrXXvwloPUq3AO6r9%2Bq1OCYYLT3TWxj5GSjxDfonCDHZGKb3hyvOE7hfcaFITD6vlqgBenC7BuvOQ0ziJ8RhI%2BdQb0z%2BBf%2FFUb8aUnPI%2Fd57jchzWG3kmMyD4%2B0pORmkXlZuS9ok3ymioYiRRc1luYZm7CAqfFtSEDL6nDdeSkejC%2B%2BPefGaUCBIsrE4%2BUZzzdgp4W3p4D9g33by4XRJ4QbUn6%2BplQaVGGIrMjcs07E218tvdM4eiVUT5Pd6sVOnv2xGSHf7fpM20ylrJUB%2FlGj%2Ft6Vvu7DQ6jnHy1ZI6ShAZ%2F%2FTB0qAzSozj5bl9Xn%2B6XSz%2B0eiXqcPc4q7H9TnVcgKzPAWKbtXWBbMUv5Zw2tjgf1g8sqFWS5%2BS%2B2frruLApBHLxsKYXkB4IXxD%2BMTDOov3nQibw1IyIjLUE4lwsz9BMwVJvqoKAKy4C%2BbyeluCL02zRCwf3xQphq2tlKpyNHoCKEFEPqI8n09GWT123xbKORpsePQqc4zrBFGvkI7Fkk%2BZdiA3e4ZOmMMkvuW7rWkIwv3CYCC1R66YdzXnYtivSs1m6khS7ulJuW4kPLLWLai87SGqKzXQEwfSNJAOnkam7jId1fzlINlxU8IPFYF7PI3gUXMBjUtd3GsYrlkugv9h9mwjDJnu7IBjqkAep1t%2FNeoNsETp6xTNWA5KMqvZaYmf5uAungh70VNmEON%2FLP14Xtkz0NzuvZKifX9sk6kFqUkoaki%2FcyB4YUjT86ki3akCI8QaUDdJ1fWN6Ax6uCRuJhq4b%2BJke0rpa6Nadl%2FWLTf%2FTnmlD40GYbXWMgCEx94M1sqV2Zokqg4sEpKYu6efJU%2FYxxnUeO9qNeW%2B98vroT0CQnktOF%2FUZnaXs9hW4F&X-Amz-Signature=8a9179c923370cd3f4032f434e4c9bf820df9e31c666d2328ea3c1cc5c5339ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

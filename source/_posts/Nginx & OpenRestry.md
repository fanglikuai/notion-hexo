---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WYFTRCTK%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T160102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIAQyVZPJdXive9E3oQRpJtsO65k3NLdFX7GR8avdDio%2BAiB6kyW8imm979xNM%2B%2FcqXg2tU%2FjxIeCI8DC%2Bbn9pQMHUyqIBAjZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMkuUeTPR11t5PzSTeKtwDXi8snyu76e1N2o0OF5V%2F%2Fjdt6Y%2FAmIfBdoBv5kBXmbftqdyo9xSyD%2BG2G3n5QoIKeWp%2FLY5HiD2e2t7E8hdCtNPnMEYEohABjIl4HVVoQyI8tJSmr%2BSjoGQEgpdjs0Elr52Ba1hN4tM2gfPE%2F8r3WTPngldFEuC2KznA3q3kJIxJk1WZJHJS2wsIf1y9SEbyxnZrUrCzU87sNAcgS%2FzyZw4jya1pBCp4Ok%2BU84OzjhgnapJLaJKDSfeRGudjK0nAU%2B82S1S%2B0jhOyJ97Z0ZBRQAJcLwky7jf2iu2B6xmxZb%2BZxI7bzvXRsmRzKQcRXFS95eqcmtRXtHO02ft8CJGU8vXNW8ENoU8qreKlM0cDyeSfFbvsJbY7y4DuU1gMj2E3BPP35ywj9vry%2F7mHMUqsX21gOK7Q4AC1mmoQ1uN%2B40m4QfIZ4KqEeekGFdC%2FmD9jwfb5wKrDNbjO2Of0NdfZ2sSkja8lpe5VV7ymrHsBxvF2p625G9QagueteWvIgWPDJEptIN057SmeZU%2FOJp%2BDug4snawt1KkO75xO%2Fy%2BlFS54QbnR2u1c1TwD1ZiqOb8VbXWTyY6Pn%2BUKOyopSiYspfU78ylr2pc%2F6zf0keD%2BxCEAJ%2BclPmBaQk9bFMwjNXqxgY6pgHFaxatAtIzkdzls7nkq%2Bl0QSKkNqfdFefVSeF0RsarG7PIgCs20q6F2Vo%2B20EW%2FPdwNtY33LHvGcusFDhKdFduhiz8geyGC6N83%2F7DA9f%2FNdXmAYKDDaXJEexBD2uvrne8X2c0zmEqY54CGa6WIYH%2FgIT9D4hSXtxwlknr73QHq0oChwK4AXQvnMCxTtKMkME8gazwqAvDDliqDPUz4j9zgA%2BUab9n&X-Amz-Signature=6e3c868859af5853b339694a5dd58b9162b705210387c789a29121dc3c841608&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

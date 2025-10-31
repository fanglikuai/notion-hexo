---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGQIALZM%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJIMEYCIQDW3%2F7SmNiJ6zMG0BhqXEs9721zboNmbeSIBM9rTYe9sgIhAPAOghTHKIVoZHQN4NcZWcl%2FVWXsUQiucQjEf2eqj9EIKv8DCB4QABoMNjM3NDIzMTgzODA1Igx8HjROX4CWXQMnEHwq3AOIoMIvnZemNmFMr3H6CJUVnw%2BPrYef5e3KaBIbSKlA1rhk18uDc9Tg67OS895DfcqVIiODhepSq8VSbW4B95WKzW4jQ1DkhoQAV4SUq15PBIdBCeZq%2BTyMqDfllnymoGH3Dv4HMSCLDEUQad4VqY9eEr8bEfTmDholh6P%2Fgr51%2B3TTp%2FME8Fq18B3HLOOpZXI04ISzP1yUr9YQCvzc6eVowcW0BgekEqoGYRniPFewlTZfKgOWCdreLr5u37IpHdhzrl%2Bx0w%2BRm5X35htKzWX%2BExDhwjDFqQLU3nCkzzGn4%2FYQZlvB2T%2BeZbrbQJRguaTRVubXtzuFxh6CSJ7I370cy6MRAYU%2F%2FrPYQmW4%2B6xjlgEVpB%2FD5K6CoOjZCerGEHE54ldM1gWrW4hfAvNdDCc5tRsJm8ufA0%2FBrxQZZ51xXOmh66kpMhnCZrTREAiJkiGuf%2BhgBgA7CBzZX42tj33AWk3%2Bvlb1vgO64fbjCQO0qbxjeKbYwciLyK6ugBy6S2RJW%2B5ubI8yRzs8IO4nzn50KZScTqJgDZ5Ldza7FzVielmwan5jliKCEPvgK4ugx2vYV3tfGp%2BcoF3BSCWRLF2mbjjBvPO7eWNcbXoyGKwgmeg18GaAv1zrM3KtRDCIwZTIBjqkAedsqJtbIQq9Nje4qnRcs2%2FTsgEp80i9cyJvIHFE006LqL60DBI1KC0XZnjnbNdS25nQSqxfVeOsdJ3SMxqD1uIQKoAIgXEReXhT33gp8v%2FaZRNqxmCSgUWnx5x2AVATpEA47I5CcC%2BfOOYjNnPsRQbDeMeXXg2hh5a6cpmc8zo%2FOdfRpRfSVzqcg54PUo6%2BOKdtgHEdY7Ge0mWniYBFNmWrMGQ2&X-Amz-Signature=7e91ab5beaabc030e8147bf12a9e0bf7f18f27cb1d7f037b6894ab195a6f25cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

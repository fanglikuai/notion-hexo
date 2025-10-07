---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VDZESLIR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T060140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIQCvq4JyN%2FxH7obRSWD6qUqW%2BwOE5b9yTyRcZUCYa%2Bj22AIgCbZSv1RAJw5s499Ktvv4teSXqoovYfMPCvuqIm2X5vIqiAQIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPooHboPAvovkYAPASrcA%2FBTD2YQSstk%2BGRVH03I%2FhvM6SL0ztfkL5TThXR6cUDHEcsj37zg05dPwcUzsAENNshJZ6LYBqsRwYwOR%2BynGBb5hFQp1GC11iP5txsdsvehQWDekC4ZJUkIKHYK2%2BsrP%2BWlEZltWAO5e9XrSzHdhhrsmE26EuYhAHxYZVdZtfSqyg9LDFdCf3GQ5UNwaSfEPCJ2HI%2FuEwew4Yi4qAqbpaAwDJJF3GKowDigWZfCYPCwFEfWQwsSdDzdfo%2BimZslzByhZ96SkfPhO7a9268QB9eujMB5%2BRBfPIioUPJJU5%2Bz7DtZtqo%2FzV6BrluG4T3GBYt7%2F0s7uMuAyAWTRbtDjOYfjDPDnN%2BBQ13w5FXaFMVKK3ubHwI6av%2FmpVYJwp3r2meEYL4j4fo8mc0rbFSGvRMj0iZa8bfim4NGtXNgsbJCG9lTfuTizj%2Fydk8W1GAXwVBWwQc0S66PN1Cx7%2FZquXC5Uq563rzxM1ZDacBa%2FSQaZHcSbUhzWiVUmxv1C5sqBhueQTGHIQLBT5mlBjVzTync%2FU87EoammUdGBdjQosjWBy2YXr5%2FAiqx7ChHXSa1GhB%2BF6UaTYYcvsgqaGu9YvJjUHtm%2F01J0subgUAE5J5D3GoTR0WufghjInIMMILSkscGOqUBp8LyOxkWoXLk8aXC8QQXzxpmtpALslBdJ1FJxerf0wzfradD5DDT5IEDQvWC9V3Wn%2FqeR4HxMBAohlvCGpdR2BL4hSl%2FKa3gpsSRB20oUuZgfoETHBEm6UCf8D1N9JGMP%2FUkx0fcn1v7%2F8IQDHrMtO8f9C6YDnylhMWtjhOUBieKrlmMIRhFDw2qRu7FaJlaNx4NTrlbm538HM96DqPKqfM2Pk6Q&X-Amz-Signature=d7e5184d320c52a8a85d4369e1628b80b4b7f1d232d6d80ad9199124a7e9934a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

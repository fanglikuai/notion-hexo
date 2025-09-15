---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WY552B3L%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T153712Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCv0Bahu2zKjPQax%2FxWWrRag1ME8DyZzKmTWtKQRRXRPAIgf37O%2Fhgdl70yHHoVBFZfEItZgcf72QE6Jc72Pa4FEt4q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDL9VoJ60fMA3H0wZpyrcA%2BQFkO%2B%2F5PEHboGeno%2Fevi3X5Ay4yirBAmsNz97qO8IGbX4tEYG%2B79F6kS%2F9UgCnk5F4F%2BTLcZ1SboH9x0k3qBgjfWljLuRFOqHaEoNu8PsQEtwPsALFn983kCglc3qdsR%2Fq7ZhRY1NYlONJfS3m9sUv9KcSCjpqlSTZIQPES9PI61PSAWKQt7qapcMm%2Bo6mMd04jXxEmBuBqzZwdtNhhiPsAOiBG7qzJHmFY%2BMD0VEsI0o5QQnRa4fajlVybVo0d1gIg0YSpEHVe60wyewnHg0H99dgvFYfJWfl3VjLw0FE1Og7gViP8Jwl2Ih9XmV0Zv1prv8MC8a5xq9LJuHdI0YGhZ1vG2kOWiSiz09bXSYu4Oe2rSSEcobcX1jEe%2FqZ2TzylYt5D3OjQ22vObAnlHqbGKQW9toaEfPS%2FtjC9e9g8WFXKEdA0kBhjP7hFsKjBZFBFEvNZIuAoX0c%2Fdv9fujiu9mUSQRf8i6mUo2PJmrDUBWNR0zhyQ2hIBLoRQBtPHcNXdaXOhVOmfbPCGgSW6bKKje%2Fxlh9nswt8x1TWAErhHXLTAfT%2BrNHhuBsihL8PWQ79Sgk1qnf6z1wTPsDKJgfWX3WDzkcaQ9OLQaesO4CP9FcPJcQu0K%2FelIYMIbboMYGOqUB2SxFBouMrS4VLlsbe7jDWUgreCbOnmcMs22cCD5KCPm%2FFx2ct5tkd4nTcwebUHB8kaoKg%2BsEvHR8l%2BJJdT8zfattjvy%2F8CwUbQpBW9bSXl82%2BZG%2B0Ju8LfnAfbF%2Fi0LiZ%2FyH5xu7Igb6rH9IHT3LABqSRKOJHvvwZrW0iJeA6BlcOR9jzhOcU2v7PhDmrtdR3ZGUiSeT1XgpvQpyA80daP86to9q&X-Amz-Signature=05dcc92785ae88c7e0fcac02edb1801eb22dfcb337622ef8489c1049338e160c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

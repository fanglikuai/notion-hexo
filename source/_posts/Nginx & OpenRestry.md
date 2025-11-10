---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667XZVDR74%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQD9paO6cYG2y062vF1QpG%2BUnN7Tg%2B2Xx3X1IyBjyIOhAQIhAMQEa67uV0QCG7jQGQ%2BOmqazfDjIH9VWZ7zCSBDy5v7oKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxtvN6wqZm5XDm037sq3ANNsQHg3c%2FDaMcGroTMdj4LGegwDBi5w5rFP1pHXtdQVAyFXl6hCUrCwTLsIjDc9Ou62dHbznLj%2BYcop%2FvDZMZNtZfxrRWEDY%2Bi7HNlrZ1b8xdlS%2FxEGIfxZjpqjUUwji9OkIy3TCGfHNvVh0%2FsTWEgm6bxEvUE5Rf4YXv%2Bxwurd0c0Z6OaA%2F1ukCeKBCkc6eDYuHhwVQqLGaMlwEOXjtZQ618UZW4qzhQxphLXaIbtlV8tqrq9uo97QIoszs9PsNLoDrpEkMTjHH6ExBiBqVEZah5wnokax5iSItDLodpiDwHi4JII0IGOvz%2BWvaQf60ktcGRT6%2FwOLFSWsokGojYFjSS4b4movdqc3pB921lY8e1uRiV3AitENP0d6ptwE4j3i944DNnHhRdJK7tL3W2bIbOySAF7WljqGXrVf9ZLjALeDZpuwhjAHwHJ5ON8vqXwMbZmShceiGc9Nhw3UehjsWfuck6KVCCkuZ2sdFD6AfpLX98K%2BH86J0jbJ533wuJZbMkcsBS1oG7tUP1jfrqCFz%2Fqnu8XYzejZER3d7X1cFoK217IhRnuzn%2FqGDdcFEvvx%2Bv1aH%2BwvHQqnOeB7bDnbVqsJdWk5qgqWb53LObXltfDCf8InnlkjYsOrjDN78TIBjqkARqIOv869gbld1kCjWyslxMeoiClmsZJ6L4azjpiumVo2LX7ExOxbIQPzGq%2FjewBOHI0QpRWrTZgnG9ZrQ0IQvKIOmOchRyLHnZHRs05tEJt6IJEfJfxUWvJd7R1nciHmQ2IKEP%2FznOXwn%2Fftml%2BFf%2BC1gxTbKkIMugOCHOkJYEg4HqAyUU%2BX6lrkNO6%2FiHVh5w6SgPRo00AmBn2PKQaqBLC69R9&X-Amz-Signature=9ba764cb00f983a396670604f8e5e71b5e985f8e1e0c6a10a0b747188a7a4cd8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

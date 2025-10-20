---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FY7ESTW%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T090708Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQCGifxaSxWjXL%2FyU5AGT5I2wzCLFraG8EvbVBDCLl6cyQIgcCTxCrif9sW%2Bu6pGiixoEOA5ceF2XNvYnlX1vJzYE1EqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeoMNueR%2FnrfVKGBSrcA0I6buwmVPklhy47dTqWJS7nNmVC9z8Z46jpaR0Co8YxjHXEt4D7PX4rPyjpWcWmZByDYRn2ALUqUl0g4O6yHepjtISZ%2FVuppXXR7lCyIaj%2Bu0s3vjWdWijZSlJv%2FabQUdNj%2BDPTgIEUYGwgU%2FZvWanicuW6nmtXjQf8S9G3Cfyb0gldbZgnYKFQakOJrzRBqyfGeWCVfjttUuRriGWUuIU6wLadlAOaGNyhJmQUQQ6DD0LhY2eB9g3xpg86dJ9ItQAdbMT2lHriT9yPgLm7DlcTOVlRjYkDZwfNnhnyeczNT16FMZ6iewiRIejeXJqDLvaNquiuYKJtRUPHfFQC4gumYlBYedJzHA4XhAy%2Fqb%2B3hM0PHnGt9jREUQa2NZCGOnSyTSwptO4EoC52ylbK7%2FNAh6Um7YZ444StZXbZmRUYFgeeshd1hcE%2Fpmp1kksmiD%2FwgHwkb7Hg8ZoJkUpkJHhuflmLZpTYyIRwCQeiU191gO72ZP8c61GbxXPAnslxqxyG91iFgquLII9WauSbHzJiZws51v4qckqu%2B%2FF1otT02gS%2BDZgzY1tAS2lrJpd3VWURbGPWWOZ5BH7uywVTakwai4IiuqYtCQMh9BQVrnHyXC6RCNEykAFWmAnRMJK218cGOqUBP0VODZWzoPmdIN%2B%2FVdrVSSamfVOCHyQgDuB9x68EZB5lM8KW3Sj3WbbW0PRbaEDWiRpQzPQ9wS%2F6FzZop81k2tqMVUcWmILsCIIwlqq5AuTabCFVLbW9sQY5QNFoYoYCgZYd4NbIv4j1BoRWde5SzpmwhIZcF5VQYQzq0DTvjHBxggjqwpP8iaqE1KvoZ2e02e2g60wpiU7POQ7g8rIgj%2BUWLg%2B4&X-Amz-Signature=fa54b194ff56f35f53d058a5307fc90185c4f1aefdc1a99a0e630b96fe0b13a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

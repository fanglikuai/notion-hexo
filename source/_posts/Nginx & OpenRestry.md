---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665Q7HG3LZ%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJHMEUCICnE8TUCpLgCXyFWh1tdvm1NJ%2BuJeQlmIl8Q4g4hhYOQAiEAuYBbdiPNbTzI%2FNykqbeBMRbClLpLOGZZj%2BgyD2x7CqMqiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDC8WupoVncujYseEUCrcA%2B91FBdHf7Th2ZwXLz2qWD8ZGZhXBjWv%2FsR8QvyxLkdc7NnTLLEe0ogXGqHcdPlc%2BbdYDeZKd7grzbbkaa%2BkKYzO586rAWbquWHT%2FEUQ54hwm%2FSzNZfjGWSo40MIY%2FUa9WlUuDXvpZBfZvkQtFblwV%2B%2BnL%2BKDgGbXzuIOBRk5erJab%2BftVGE1O1rCvgZoAw4mDsnWSiI9e4J9bY%2FM11BGsrlmeCQF6QXM8tkga%2B075CB6Rd%2FHbbbryjUPk9DvwoIXSYrO7JAjYgOrpIuJuX6cROCTwVxI0E%2FwqIIPFFBwjUFrABjBI5e0zhlUyX74%2Foy4KUF4DJIcicCoWs%2FTEcgRqzH6VJjTdNjJSkIsa9qfnm7W6kvGtCxO73MpzFxx%2FWPjoHD%2FlSGxGkLoZ3zp4AmwjhX5fzR9%2B2joe3%2FMloq7DJX0kvMLCHFzST6TM0bjzWI2dokqjMpdtO1F0lOlr36tinbD88aGJ7kXr9jFEPu5jQTcNo1Pu1kJXTtJyJeSDSZEpn%2BVMYm7p17o38dS%2Fe%2Fr2bn1kNJNQNs6vNd7I7CC9GMQv72QcKNQxjWtOS1QIsSx7Ziit7QIXBTI1uEEw2hhF3GPUoEoSbPABiE2w1uzHTpajFMROogjVQHR5SHMK6z6sYGOqUBwSvqEAhD73NhrDZqmoejU1W6cQ5DxL%2FNgY7PyWuvRkreEv8uWRpE9KXqjaujGD6SsNy2IAEyW39BJbjIjKrEvx1bASyOjxeAZKiD7LV6DTUL%2F0COPrcxnb6YUN8j41p3l9zyKXQDF6CxZRaYvdXzJwQ9GCDTFtNV%2BX1e5f8Qar3KndZ8d16JII9xsBx37biBiyNvsG2nW%2BZzh%2BWPELdiPDPPS4Fm&X-Amz-Signature=8268641823ad97e61a6049fb9f325ca21bef2754ec0be96d76eb3cd4256f6935&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

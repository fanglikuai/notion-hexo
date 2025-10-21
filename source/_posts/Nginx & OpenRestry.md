---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRX2IQVU%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJIMEYCIQCAqsON28iguTAZwTI62oRJp32oWltzNz6eViKLw994rwIhALMlmlKK8qPlxqPTMhFMlmZK9Ugie2w3mHNPwUwLHGs1Kv8DCBoQABoMNjM3NDIzMTgzODA1IgzgMLgh4b0L9K%2BlpZ0q3ANlIjeu5nuq6Vl3MgD7qfHejnOopQ%2BZy0cY6eHOhY7bqLaU1%2FxbzccJ7nnW%2FVjw%2FTsNAQxC%2BdzLsK3EgH4cMiNBxn5dQwfvp5lRql40mdwTydxwz1WNX2rybsYjPImD46qLZsovDOVZPf1exr41iJ6aQqWLvzi0j4fBBw5as%2B70zUP2EYLtTSAq6hu8FRcKaqy4ti7pf4zixU4udYXyvauuqudy1Mbmvk4LzWGJ4LkVjA%2Bbjhbe6l0epAz6e00IFF4rE7PSJ2i8gunQjO7AUwbYA6R0T7j9pfUp2AmysVAYzGy2liQe2U1xBOvK4JR3faQzxWOiOGgMX6LWBRbblNjqVObUZnoh66GFwvPjGtRASvSeUSj6SOXPCy6x2BGc%2F4SOza320zTsNI4Ixx%2FTz4c%2FieVn7b2skbCZIM7%2FX8NqHpAJI%2FV2r%2BBAE9LdxTBQpwHhUXCfJ1AmpF8QohpuE73Gqu%2FsBvFObZ2yxceba8dpjUYr80N%2BpFTKT4oZMaTfstyXUbWWnrY5SOeuFS9pjJiWSC5Dp9Uv19y30oL3lMkrsjllUFM%2Bkk4x4Zq9Wm3B%2FfHnJCmjjIDbob0NdvToqMTHRLB64Bepe275HFK9CtodvYIQbAOOPMZOmHFP3TDj897HBjqkAS%2B1BspavBnClnJ%2BMq4OwXZ%2FPC5b8Nx5aNSlzoa4WoS9OGDv53QQxVEG6Cggqm5sjRwv21uROrg4hD2qd0semKJZimQeYjnNEKBOUKwAv0yw4OXcMJLABSB6xGTgBaCa8T8iH8tfdQAXlRQspVxWNJSuO%2BjcY3Oy7RIy3D62G3qxoHqoKles1uVcslGZNgjGIVnH8%2BxTaRfDB6UG7wZXdqnu46gT&X-Amz-Signature=6103c027ae49322b6ef570cd90cb58f8cc0cd5642798a8967819ede59f4cb8e4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

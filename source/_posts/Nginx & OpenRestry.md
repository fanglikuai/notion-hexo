---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UXWZBDOX%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T010038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCiL8b0DsNG1%2BWrwIEWg7AL8eR1B4XIElLDxBtn7M9TbwIhAMYXj6%2BHAurGVEDbv6kSKozoYAEC7xeWTbypT00ybrYtKogECLL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igxv23GSBHpiWvSATUcq3AOVLcGRPl5RcoT0fgjRwoiFzWso9MJtFgOdIvMMX%2B%2Bsicas5viMSDvM7614FqqXr8WTEuBvkVlQOl3EQ2WTioFwxOsejjMJ6g9Z9IzqMM%2B%2FDQCIHQaM6OtsrpQ%2FCE6QzvXhZvmOf%2BhxyijUdJ40NbwZX2uJEm2lF2Xyg0FjKy%2FX31WbLPUP29KlOnUJLj0tIF6kPHF8%2BFqFTpWk4BSIObMtTEG1O0Ln2Hzy6889XKuesW%2FbPs7RtWM9JE3xYv04NnH4475tSTYBZ7tdoGfV0Dwxg2fXoNCDXjHsNH9k%2B79toG1kDZy%2BIhquQhExl0WX6lZovVx9asB2NY%2BFAtrkt%2Bsx3957GI1L4mKjdqI7GutKBVxGskqu0GnVtCTeC3LlirWY42JflFHEQQKewbQ8fhqO%2B1nLOB48q6mOUxw4zu9djCwi132YGxPlIHgCtBuOY8ayVShmpT1urRwbPyj9FjFEtCQjmpdMWsAIbitTSSGNWLDsJJHhOhUOGJoufmHERHDZL6%2BOCjfN1NLuCN43eQAfwQpOhFx7spGVZgKBBhykUngfeAiEQqf422LfMRhk1zUGl8PMag2UTdBLhesGB0DhoKKUhbDAW9%2FQ5HY6YoKk%2Bq3dVbN4AAAJ2rw12DDPgbXIBjqkAUGOaB%2F35QOzzY3S0q6cDEpPEdXkYiTPSzQbqj8oo4jMvvPMkWDCJMtU7xVG6j3D716ZFgSCzh6ju2s016ceBo4Jwy0N1bMcnYW9%2FZ5w%2Fy0vYDf8jzLM9U4zSTP9MUN9TpznxBJlh1FW5Y7GIz1SQiq3u5WhyqfV5o7so%2BFv6Flb0p1WY6dtK2WsQVY%2Blc276O%2B9fLzW3NVRx60c3MXjSOBkkdz%2F&X-Amz-Signature=7a3e718205332ca7714d1966e74302b07431980526d9d1c89f79711e86315a46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

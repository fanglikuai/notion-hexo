---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URP2V7W4%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC28MriqqGkpa6fIoHqPFjbylvHSqFNVx9%2BBEXjXonGYAiATE7tK0qhzdi0jePP1IFovxvIC3aKLVRkZScazz%2BmJfCqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsEP7a%2FutS64TAF3KtwDvdBgycfqwaexCLdhJDLhnJmlfKEkWJquMnals43dhukhtZDbjkmNvi4dGjFkzW4uRzPvF7SoEjZy%2F%2Bin%2FaN0glnaZmW5lBU2j1GLMLmfcbLkH1CWu5z%2F65HdkiMXUCZtqJk3b2rjq%2Ftw%2FmrrZX0Ni3jwYIpjRr%2B4x4uPY45WaFcvFhUexRYYZA6heQN1OBN6T8G35oL%2FkCg7lhGgCjhnq84Ln1phhNZyRjiycx3kqeOJevpS4ZLUxirQpIBsPnf9MTBBZC3oK23%2BpvcF1ov43eSEZI0LFk0fWU%2B5o8BHRmkMtcaLMBTM48EzCpqDr1ywVas0fpN099V61dtXMx4ZVh6ZlRq2e7ckD7iWvtS%2FdEADcsofHjAOXNMm8t3j2Y%2FoizKj1fNWqV2c7yNOMlJasGs3SeuKQpPZk9PUNOjllakckNkLF0imIYizHGag8qfw86yz4FpqO6dAgc2%2FfC7D1IwPIcrhWSgLTn%2FdIlNU0mhNUChtvA%2BcxvjfOOmMerUsQAGsFmHuzb9uduaMDs9h0%2BZN5EwTjj%2FiDuO1NU3EfKDre1TOpzPYtSydz8O%2B3u3CcJ3maXNZFMhl%2FXPBItsozbhhTa%2Bc0cARrTqymtK8Uwk5GK99i9IxHXFeqiUws%2Ff6xwY6pgFawQYAksOQEdIXXgkGmo%2B36Y52pR9qs7u90BZqMK6KK58l%2B9OAsZ2un1hDfNn6vJIFml8emTZOJrTvRaZGFmvOWgllakurYyIL1XfwH2EGmcvT2KZ0%2F5Vt8KmYlVDQbLb6f%2B%2BJ1cy89AY%2FV5g6uwlWXXPauvdOMq4v%2F1FG4DXJXIJEJ2mnv5klswayltBdq0XQdSI5tPGM7VufvFgfw8YvycgBoq73&X-Amz-Signature=9b09e1f47ddd9972b891f25ee098ed4612a2a8add5ec635d6f3e2fd4780d1fa2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

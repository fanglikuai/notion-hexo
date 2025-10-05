---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFFVRPQ5%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCUImIznjk18P0OLHKvoNLIGL8xZgfGzT%2BOS8iDAOwxVAIhAO3ptfwsGoU6PA4FQ1WpEou6eZL%2FR25AeuIpAhonYoV1Kv8DCHoQABoMNjM3NDIzMTgzODA1IgyxF4QB8XMevybRmHgq3AMOFK4N6BCcmJgovlJmWHNsXEEsM7SKCUDxmgxzP0vRozzYAvTHpN9ZntdOXjgV28De2JqSCQwfRajWlQfPfyN49gJDvdt0mYdZ9BiECqeijiBl9zEN3%2Bd5Zpn4BvB9az2%2Fwsv03XwMsvrb4nizl0xsLBGjRSt%2BbyoChCfWmsSzVbOyEZy5im67ueHYdZfuLifkJM1o1h0M0PUT7kN%2BhrU3w1Kwn8Cx7T%2FmeyXTLuyCu%2FdTk5csCOHpoYoRRTPN8C5EZ8uK5PSQ1C6CqfRvOs%2FCFntdYqiIjfuxJpSOLQlbNXlbZ9hNtzWGyiAqhYkaE00VHhA%2FbJYJpmRLr4BZRCEhDIrEa0Kuuwy%2F2tIdt%2BX5p4xY8H4ZKHvWyUSe7H3dlDmTXrxrPx5%2FEPad98CHG2v2eErHWuoU0JuW%2F1h%2BRzC7QD217Mq0%2BV0reiWS%2FihyP%2BjEan4e7d8r6RFjjniBAKn8k5Q7Qm91H3sa6yLDsMWjFmFhQPZX%2Br6k5CpIhvwmUSIVFtF1FROEOnZ%2BUaAV0bdDa15MWx51vEymn8blkYQjkZcBWNUPi4fCnjfwY%2BikJtLkyyVsZLJLyzQOXlu%2F4RO3blotFp%2BMxcBpE%2BA%2FZe20Zr4ZbBBol2WXWg39xTC%2BwIrHBjqkAT3tiFqvnRYlHIKuX4zNbaDEC%2FL6HVXPMOdgtBvRXJIf%2FsQGRDhSuJSHqbo%2BeHsngLM28wLhbTqzC9RW%2B%2F%2BPq5PcsdJN0RwtMVxOa3aYXAaYkHhRgT55Yylfc1A6VW49SsvejBbSntTkwrlRVidjBoDOblsnBJYIcuGnq%2FHtQrSYszYXhw9DfjstOLH3BLuvx%2BAsbUFyZvRMwKVn8RQ0IsF15pir&X-Amz-Signature=5ee3b57e347fb3b5c8e2b88bedb520c5b7726117626644f43b35107cbfaf1a85&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

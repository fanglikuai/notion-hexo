---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WVPC7KLB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC9zGmgE5LUUTy2pbWisTFJzEJ7HZD%2FbRNPkcBHjDLChQIgUEsAet5%2BIxtFBPBq%2FcwHhWCqRgWjHI%2FwO9KXFMD9D98qiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNmBSSWj0a59XupbkCrcAwjTFvJJOwhB%2FRZ18i0Sl9Jc1j%2FaFY5o%2F9ifiS4eOwG5jVOIV2Xpc3LPwqEGTO28THWCaWq7e73TxN7MpgnhT6KVlH%2F3%2FgGjlRqfGUdAIWhVSB%2BBgCRvZJ4490KdXDtxhFAo8T2yLrx5AY9tqutRkySrmJxea6vwy%2BKU1MGCLIwez6zrVYQ8S2UGUPD%2FmwwP5hhXLDQEqQgL2bC6zOWgo3uu55H8D3N9sZMCns%2FMJGGqjP6vbLcVMBaiiARJE3JXv4rDw%2BaNF%2FBIm05W1gwSMKPzU2o1ch0%2FuvhS8J4l%2BwnAJZjssiQOyfP5WTFezYkmg7hmX4ZXXCUHhBlzCZI5L%2BA%2F4DZpfrfJTiVmAM0yGKIXABccgfHEVZ%2FIb2tZ%2Bvkf7yqHfneLtIQxIPp3zMaVKwscTZ6FamZxvseXXWNjrOhXHKOYrdA6fw5i8dtmoPor9MlrXIartULufrKkDqJz%2Bym5Nz%2BY3w7rs4FNvbje4b6VaDyxicmTRRU9z%2B9ALM0LoOLw71CiN%2BDv1c2qxjKlfk7oNnYK4jJpIsmaB8S5tE9lKsVj1w9Pi5FLjYjH8Hg%2FKJr5xU6wDzJazR4VqeM72XM4ddFcMkvAOXJe7OljXKHMJjloCd5X5BmHKcxbMPXEoskGOqUBfGymfTUBqc0JF%2FbpvQU8XixXex%2FQ9ytskO2vdDwJtnphYuN%2BwtR6scLDOQt%2F74GFcB%2Bj4wu5G4H9YOqpZebguzsoiNL5kZDZiTGPWLuVPmSbTQKypNjKNLtfspx0NFZuYfH5xL4m5DN5YzgS5AhKAVKqGKfky9MfO10ZvV1hwWlLl70MgiL5iZIkVN8B4EZkSRm4pOiQwOa%2BzcwCiCczrMktdL2L&X-Amz-Signature=31a95063014ad14b97c3f2b7c18849f242be762e7aedb0b5f1c9e8630e0c9d57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

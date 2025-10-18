---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NHHQOTI%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T060050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDk1590Uw8ivID3U4wWQXXl0Gz4qIhYnaOA4Je5MFWH5QIgF85R5YhE5tlhapXbY%2FJKVDIUMLRUWoHvngoE%2FmRO28cqiAQItv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDIxykdaP7e2VQgwACrcA4caMN5V%2BLoGRhJVxcEN2F78CG1AV%2FlYkO%2FVS4rtOEzebYwJP8zBeHVBY0t5vpuphg2qq4O97XdMzShN99dBt7da6rmC0XrqBrBw3haaoRLpiXzXEYhLNWomCm22RZ6B%2B75uvW98iQUHEseIwwjvC4jw7yQqJH5nCOQmcpTdC4jdslwuo2lIfmfaz0fphU2TQdj2PHFA2gt5V0S8AIiFDTjP46Ka5dRtgyhxIdXL3mF6W5v%2FtC0XAaR28PCaLsAAsrUvJsYIilzhEnntu3uASvjtNSYhBXTECH6V58WOR9RtsB98cnptZYHFqw3tTj15l5A95yyL25ELy5gx2z%2FoSvb7647rulWRUeWtDCxnsMt1Q4M5xigYZuuLYMHVKiRrb%2BZZXtxm19gyJwKNhQ5R8%2Fs3%2F0Vs9D9IJtDW1%2Bm0dre6BBHRqlHcVpeZxdOyv03UZ%2BKPG59i%2FPl0kBrPXRarpdeaQVUtmBRjONIgBqWXw4%2BEqew3yHlDajlU4wS1tGxErfb0B%2F%2FBUlOC5TgjfIOLepXJpt7lM15kB76ZoKmdjJkJFlUx5PVi6Avky5HzHWmmLTHxvqkSIHjy3q36BGX0HmqHd%2F12hykzzs9TBhj9XwILzjTGw1ydBCDHqhtsMLvDzMcGOqUBjVcMtlaFlYZcyzMdk%2FHE8Hl7tpe5EIIDzeq0tPzEX2qvbRyU5lpk0hyliY%2B75vVeGno9K64fEeanibkqNjo4pmRHE8OO%2FER5E12T8VNro7icpnljTGmC9y2fvdsmrhvpD8M8O%2B%2BtAK03ToDLKVpfLL4lAh%2BjO0TAYFOAjuZTQq%2FPd%2Bu3FL8H0J6%2BU2eknVKkmk82TfO9JWdvrfIjRxbSfWOguYBq&X-Amz-Signature=1f028ce3538535c22051eb13f884313157cf61a8560359f9c84019680e79a462&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

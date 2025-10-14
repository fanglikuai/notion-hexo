---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667PZPHIXY%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T170119Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcPCZ3oySTOP9ZTHAte%2BRsKhlszUi2pwUuv45v40R6SQIgR%2BNn2b%2FccTdR5GQUv9ELqPanwOYpvMvsbn8xjzBM4uIq%2FwMIYhAAGgw2Mzc0MjMxODM4MDUiDNXXTHvFTcsbiaxC3ircA612EMTD7JjiHgINAJH9SRnJPAKyxwABSSDhQ0oMr8t9C3NDM%2FzKJTgasj9ywpsiNEBefnFO9tkWdgwFp%2BqBJkBazaIGbo940iOeMueXNqwnQjL%2FHQ8B7nlQ0ch51D%2BMzSIuWWrnBnCXwAEWawner4DXjy52tdfDlDC5DqPxMyr9447wX5Z5QJb1%2Be%2BUraxmJTY%2Bw6rzHr1YAYhqe0HqjxxUyxeV%2FACQxCT%2B8cIuGLw8TvL47pKxeQt673y1%2BjcFlen5%2BJBLZl%2BRKKhGBGN1sI4K8gDvRr2y%2Fw1d5NlGvd43vjtHLmiTWcvGzRP2HSpl9GW5Yqx9p1nc5TJYPtwMIvNBzcmBOfC%2FREUirXlzM7T%2BJWyrgj%2Fh5xyxg3sBYcN03oFLfME8XPjPZBJGLPiI0VekG6I%2F4IJdsmyjLj9PBcA5OtoiAakB3ZIWtZnNqVCN2AIOIZ9a2j%2BvBrKEKBLXbkr03n7eW7KTD2A%2BEUxY3daR7W1vy%2BwDoYiuklQCbNQXzfyACZfUJYPSvrDmTJWoUbNwBxacc%2BGwq%2BLs%2BCRGtX54jebPx1vuppBZ%2FmeakVgBhMf5w%2F6MwcbHxk%2Bvt7UWnwBJ5ZLA2Ci22X1Lw8%2Bg7HhLA6%2BcUNr6v2JjhG%2FKMMr3uccGOqUBRs5K%2BEM%2F8mwhuRl5WhKmbDbz4VWO8KR4WKeuwnScsVTZMjDBAM6AHvxK4Aukv6oOdTvBYlm67Pw5hwa2nSL9OYz4BY64DCVmhItK11DBkyXvJtICxFj8%2FjuM%2FkQUg7HlFkPbg7NWlQu7HwCrjj7ar7lBfmW7%2BDicPwVGj2zs8o4aMujWFlQ3h0vEH4RBgi6ulGxmvy1Msf%2B%2FvNziq3pBsVxBkKnK&X-Amz-Signature=e9da5acc2da43d53581e84e8db5cebad5c6ba94d327456016e98dc158e941629&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

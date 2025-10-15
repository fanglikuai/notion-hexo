---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664CRWALS7%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGuYHNg6%2FXJ2grbUlp6oACoyQpbx5PaUKf0To7LP1PC6AiEAra76JY%2FJxsxVz2UWYQvpN8qi3MoFyR8tccg8%2BUtryI4q%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDMY0wLCwy47GZ8sA9yrcA8wJ%2FRaynnua244pjbJ3ZZNeLkWpuCL8kkwUwJ5O06rWTRRsUcJ6fbsXgJOH0hvMusr%2FQ1QcS53HPffUqVKeQJGcrvRwq23vMAs1ylWYrE9G6c7ezzLcLNFLmthC5ghnPVefjbFUNP3HMcTOANeOltl5oZJZ7RDRFK01SHBvTVCC9OB6e0LwD3nqR%2BppEIstSn9wejv%2Fhwdgmw6qi%2BjeDpMFDafWpJpeD3jpyfLWtTP0Ikoh9m246hiMDVZm9MU9yGiZXwe%2BKHO%2B%2Fe9H33wgLI%2Fb13crM7x46l1yu29mJf1xTeeqVJkBedVexjKcAhhdLXDnqw%2FSwWUOEfrpyPxMUYhV4%2F7sIvKFC7FnCCGGiMNAJ2FnFzXQLSCMB7efx1RifL5kQhwdCU4sIcxbUHWqXxav3QHTWL0ZnurFrN73Sawk8f9RV4ZqtCZnS3iaF90w3FSTEBizbrEUcXufrv3cA%2BGp7QX3NR7iUwUjptiuyilhbTlY8Hgh9YujxUFJu85VArGG%2F1qu4FfFN5nRt0NDArhqeYassLUxKC5s4Ek7GWTyzi3Yd8EbA2xOqvQgTlE056U0qWQ09M3aB5nfxzrJVXbjXsIOVvSwsSjNiZqJXjxe1YkGMMrK4DzX5PEPMMH3vccGOqUBPGd86evhXxMHnlr3T4qKNFW0oI9AFna10MRMjG5VJyT79jlahT5hxwNH7kpA2UAKxsdHZwSKs0xq7spDJmwWAzIeHO%2BkxBrAf9QZUuG7lzDp7WEYDLFbnJqRrvxSiUIfw1G1qS6EzNqP5y0zrjKWOONa2zf%2BxNHpffp2ZS1skTSuW5mRonMbAzaMQIgUFz%2FgpsaQfGwMUCksQrLkSn9K1gN2ty%2BV&X-Amz-Signature=9209d1e2893b5ee115066c12d9fb2953ea875a7e9f62717591d82f8afc09ea1c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

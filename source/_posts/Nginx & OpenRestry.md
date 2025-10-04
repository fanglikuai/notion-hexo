---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKG5TXZA%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDr2a624LuCI6mpxruWwqi3Z%2F04VVIAwzbabwg%2Frjx4BQIgDunuG8zESg2gT9x4nW3jU6WITO4LLDXF4CmSmt4q3Zsq%2FwMIYRAAGgw2Mzc0MjMxODM4MDUiDL9N16zzKkk186ayFSrcAxF5iSSoALQhija48FImru%2FlD7AH89i6B%2FZfL5hS20RzT5BPOxUs9NcHzniFh5PBmbYZNuZ0bUvqELid80DSknBcFtJ%2BgSjXf%2Frf%2F%2FngyTNe0T%2Fcw7u%2BLhHilW%2FUHcdiSOtlsXXqZdj5ZuRBlKticCRZ6WjxQEMrWxPWMWT5614MIF44ngDOexPFXpgm8lPFp1tPB2jjKJkXZ5IAytupyR0hPc%2BTsZgWGLTh6TnhuA2haLgHctkuMjx8PocTbe6lQs%2BunR0MNMXk7koMAkpx8WAzQwUIlk%2FPC9iFmBRm3kdWY9Bvb3wT85oGt%2BP8xbFA4lkFGfQk56QZlzqP3RDQ4O3C9BnF8NW0OyKb%2FOdfPHqTnnx9TI%2BNzEO2QpWEFgjyAlxllUCm01288JMkG2oNWCjKgrEsUDlmdrqfPCxVZZkiKeqCq8frHUgVPRMO6KAAbXna6Bt8FOd2W2GmlnwlnP6lbDuKHtK0ZeQ45zS7l7Tz%2F3SoPDc2PjYDglzp5G9E20RAHrrh8geKSeWmv9BfXIdi6smHVEBQTjWsjqRudey1KoNs53in7Sfjziu38%2BDQ8lhQ6TCvGPaoxLbhec4mCkmTyPtDIt0NdGcjjZycGrdLLWpuSFdn%2FSYbAINPMIeEhccGOqUBlZ%2Btiiwq%2BggRhlCxz%2BOgbvPZOf3tdgheDZswsJNON0HhEzkTpblxC9RNsSxLKn4PGKm5iGTvBapXio%2Fs1ooCHdo1BfVKNxFq4rv8tAZqepR699PAZXFp13lC8nt%2B58HLUGqu3GQQgzRq1QXoIX795DTUWGhcQLY%2FISxFYKAFAVWByLGGCdK4UAvcZf01u%2BvTkwXttGSqHWtb0EOxcD8Q42Zoy6Kx&X-Amz-Signature=44ed25780cf2f61c8fd3da13f28349f5aa69fa7b51b3eee72b1c91fe4c2beddc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

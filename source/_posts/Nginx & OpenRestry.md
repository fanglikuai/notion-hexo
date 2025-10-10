---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673722DDZ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCm0hmBLsu7lqmJW8gSeskxi40GCZ5vznh6suIE%2Fcy%2BMAIgVBfV4UgCIgKGSE%2FEUhrNGFoFV11aSg4fL84PzOnkFtIqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOjNEFRswgCkrbbDDSrcA27b0goi20pBwCcJSRMMkxZ2PA7NHv34lGPuruXLVNbNdWQ%2BPZ%2FC26kNVG%2FITUebFR9Q52hZE1Ovm2F7txwTIeLaK4F8CxGVRWCXFX6JPXMyd3fXF57EoxsIsnzZA7JEgfKhQYglvTeiLNhHE5Rt64vNOUlmFXyHU%2ByxxOw1YTrWfgkmSSXtMYrQi9jUL8oJ1o6W8B3Mc2dE%2FmRkPORivuOdXHAO3xdiJCcfK1%2FAbJGqPkSFtDxE%2F6Pt8jkD6ExBk8ziePWftx6wS0xgQig6i1X33jwgDFhfbJIRWnii1LzFOTLWOuhdF7dkkMW%2FiWwiTfrHApUlQvt6MeO2cXKgOLOe4O%2FLSNk8T4kvvW%2FupJvkDVCZo7vIsNfFWiCVZodOZLRgJjXU3JE9Gq9tYcdv4RUzWleSzUJXncy%2FqjEiOXVR%2F0zEplB2QjjB2WsS8svsr6nOULS3WUUVJeQZ%2BFpIj8XPBXqOCixodfJUHAvwJX6z5HLwiHaRZqBz3GVwFV9L3OyDkDaL7AZnQ8pDHPCPWqpSNZ35PuTBz6611ombBv8XO1tQDMPA6fCd0St2p6qzcdCwU8C8qtW4ggsbzjBYFLuFNfm3mjdkRA1Gtw3xH4H1MzENPzpHv%2BzCcwpeMMy8oscGOqUB94%2FQzNSwRsno9abHODfgB0HbPfVKaawC3wKPkqz8H6EK3TmZk3hU8fMsvwLI70FjqTt%2BDYVbCLuPL25aMVmjsSkWos4hwVuhtBYhpj7RkZ3MtuhAgEob2PsnrXz4OVjry2KyBqaCoEGOmogVWpI%2FfMXEA%2BjK3mS4jUp70ECX4%2FLQx2WusfOBbtIEXBIWvLUbGK%2FU8ZHoX9Ey4JoS9pKNvUnmY2Q1&X-Amz-Signature=0753424cc32a2263c63285a0ee1ef6862dec11ba73faaf4bd406c47fe210651b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

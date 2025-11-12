---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKI2V4HB%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJGMEQCIANy9Au8a%2FnBMFDG%2BW153gY7rjIB0a6BB2I%2BypaLkw2lAiAW%2Fh2aeTZPqYi9VkNzH29wGN2PY3VhZMYyf5clfppnDCr%2FAwgvEAAaDDYzNzQyMzE4MzgwNSIM9%2FHqun4t974OhDUAKtwDOwHPCjj8l6p1dn3oRRaFq%2FwqV4oj5UZ3Ml4eMbp58A6Phd2GXGSXlyGKNn7KVtlnMOmBG%2FVpqvBrGBTmAKNiurcTPsf47JAMySkpKU04M04jHQxcbGmdkleusNSTVACLscIoGUT%2Fas%2Fjw3uNy38pxxxPz6uqjlUkWwxmWASWCj7dRhDLEQqNxX8OpguMB1erdt6YN%2FQgAWYXLuKIVRVCnko2ZjcZ9%2Be3LAHYyFgNzXf%2F5GKDz71Fo3hnmJhppEYqLjYlOByPYP3984R1xps9OU6Vk8E%2B9h%2Fy72FFPj5wsPr1Q0Ekmssw967BQOgP1ULDkW8CwtgZTiDp%2Ba96bTFH%2BG0nfpa6pHLWfZS73XCDxxmw5GKMMJ3oq1Z8T2UXXzSackS8fEjo6uzflaIeIjDVllqLZhfd0gfEAYW7p4qDUMK%2B5owRd1%2BdOXZhcOxRq1Dw8N%2FdLJFqd6uTJVJ72xdrjBNMH1NHJq74NqBDqaK3WfRI9XLXn9STGXtr5qyWOUriRGw%2FANHTXpvuhlNIz3%2FNwSI8FLjrh1X7N6UYjy62b%2FwFNPwJCskbKz7mHhKZOaLc7jsAOF4ZQEqrwtDHrWO5ZNGSzcZJD4uquxBRXcTAi0MKunEcPAo0oQeoGicwmbXQyAY6pgFuBN%2FoRqYofoed86TDW2AHW5ef8M0U5aNJzHujLqTgoujtB8NOo2JwULDCg3Ki4QtbzXi%2FwOZidl7Olthl%2F5rqE0YmzP7fWNBt%2FefVGzlX%2BL6Jq17al59yuSU8J%2FcNfrVTtY%2B5OGX8IQQNKXyDg5mIyrsz4eS1z48079Lvbprpfi9S%2BagETm68DcZ2GTmGIZdA7Qw17htcoKAKK2xq%2BZIleCHIN%2Fjl&X-Amz-Signature=10c651f2342ff7a3b51958adb182571522a7cfcb95f235febf93aa92e1f87013&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VV23IORB%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDTicTPt24XeEVkVoSl2YAg0vNMJBMiwv20duJwc3TZCQIhAMqJUwO%2BvhuKyQoVzxrHqk8U1fO7a%2Fa%2BTYQOD9rRcyaBKv8DCCIQABoMNjM3NDIzMTgzODA1IgznKrEk6oQFMLXTojgq3AO2QkAAaSB1PR20wWCnlmK33ic3nMW2toI%2FVvWdLn76hzaJDfenj0eDkUq626c%2BYkzNSDX3vcEUCvYdfR%2FviAjLmK9UDQS3VCtuQ6td8ua0cIl083M7CPuQsCPPkSpnmZxs%2BqL7l3UyNGFnWA2KeueNawnTUfgKUzXYabCWdZp1llX3jOUHykTEsteiJ59SPPSdfdwu7ynvzgfy0keDHNaTuDaLB1MknNknd4eFftiSApH0O3pOK7RpYxyG4WF%2BrE%2Fz2ECu6zoxWP3V0G4ikKKBduhxNj%2FeuUTPsnrFG2bCUyii9sA2YUJVZ96aYo3cRgyEZ%2BM%2BXDASdmCDyoaJqhev18VBggllrUzAzyTXMY%2BoiwRa%2Fdk0GtyBAUFxYpVhZwLUlWmPs6FChqTyZZCwl3LUNXSuXIYDnCc2sr4pdXXqSN2N%2BBjO5tJ7Pz9kC1nM8e%2BJXXJVteKA6p2QY0odvNG%2BwOmwcDuHgTL%2BAzcdgI6bEJiF8kvPmupyP96wOywv%2BexniLwPKUXlFcFOZwZS0bqmVF1nTT1f7Jlca7NRdkIohCwNtSwjuQN1uDCcBO8GJZTDshbCbhCzX5%2B%2FaQxZa%2FYJTRNzI%2F85TkybhRJIeMAEOMmv1WiFXa3q01tmWzD8m%2FfGBjqkAVrXI%2FPizW%2FfpFVY4Tp1QdgvQmMJoxzW%2BV5%2Bxyci%2FTDpAdBR%2B25O%2FdesMVAZuyTzwleChegnhCtLJ81sD92sdsUEx%2Bom8thWIjKuzCRMMfFmmh8R3r0mwcLwVa1F3g5NKl227iNOnHoLgGMnwYDCbbZ%2FuReiw8JtG9hb8ENynL7%2F8u329t7xfIK35y3hKGANWAEnDqF0BZrOXKfIO9PMnNRhpCbm&X-Amz-Signature=8b4501b12a42908ffb5dafa37252fecabd2bbd7cdc93e821c267564986052f1e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

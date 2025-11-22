---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXQLL6Y7%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDb2a5pq%2BIfxn7SL73JOG8H7xrmtEMpNpbVSQf2thIxqgIhAIHKPZgNJPeOguC4ZpUgmOdsleGu9vz%2BK9mRNnaSVt7wKv8DCCQQABoMNjM3NDIzMTgzODA1IgyDUourK5ItO74aAxkq3ANbcmlLZ3U3lUom5n3%2Fi7b7z4PQKESc%2BDOWI9nu4SNrzDoOzYJHbxu2h1ydcoOIrfLPWbQtUSiUUET41Mt5BkzDnVTCqXhimanOoMcUFfDXWN2j4swBPY6%2FNoAVYdGmkGXORjCdpx%2BBxcyKeHIMdWVVfRxYOIZpLQqrv6XdIZ4alZn5mwci0DgolSIm5pyxzZ2O%2FZ4cjLjmZ31DNsrtrIvnwuTtIrmhIfXXNwz%2BP3wtnxJtj9GN6LQazukO6EYLGM7S9CWkE6RZ4SHEyJPSgSJRTcZt71eZqpGW%2FBGd1TsHTNrcVju1VmKmds9xcfM0GJosWeO%2FefT3iKKHckL99v%2BvP0bIOEk71Q8QUzsYHX806yf2O9s6vva2EYpcOmYNJZxukRNG3Pk5dKNdEme23qRwu%2FCWBpqgF%2FNDuYH0ZNdUVRp5YjeeRBlokcPHU%2BTWZ6SfCWvWHJSKquGwj4Jh%2FboXgIml5MYn3c4cNQwHwIWEH7VKZP6ic81oHEWvi5%2BHVBcCM%2FVpw0oRb6sny2BZUo1eNaUlXQjmqrHs0KlPvAesjydsW%2BDQqCwDvZ3tXLEtBuke9Ykr1o3jfM2kV6CrGyjBo4SlS8pDbOlUe2dWVwh4rLKhrRd1PhNrVEiTbjCKoobJBjqkAcBQTlaeX3U4hl6ieql11svCUF%2FAbZj4f7XOhyKvb0QsUrbKSJpF39g6rhWGdbV1uiNhNIWtQdXqzMf6zSjbSKl0Vv%2F%2B%2Fl9sigp8aVsod9HSm0kPGG%2FOM9qjwuEkxP3lifg%2BDJ8%2BxE42shI0uuCRRT2GosgBnbBnE%2FqA4X09SdJGXUmp4cjeC2TXM0erhV3naBd%2FpVWHpcrvypKO7BzenowV3Qx0&X-Amz-Signature=bd63ecf454c6e5cfaaf97898a851b8406ee9c07922cd4d985475d65c81fde2d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKWK6XTG%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGmBgTn2bKCEBUjoSkb0tDPGN4vu6kWVoD0HueSWd5DBAiB2P6BWhGuzy7nj6Y9mkQJ6YcCVOu5LghJ3z7xk2%2FYfair%2FAwh7EAAaDDYzNzQyMzE4MzgwNSIMcbYhqDKDbTa%2BV%2FOVKtwDYkLVu6snXY6az73egpYrSfbMha4lzA1Y05%2FE1x0El5JjAJ9Uj8KFwbOWkQV9C39wJoGPvYvE36%2BsxyOeIMF7C05%2BEIUKt6GcQcvoubq8%2F2RTFaAHSNQ5VGC9v96zzGfoNJ7DLbBiEV0C7z4R5snjamHRczNox0qroO%2Bu2ZRWu%2Fg%2FKwVTHSCGy0gs3Uvx5mHNvwakqFVVvorocGJ%2F6LqFtwkjJi1o5Dz%2FKLJGrkjh%2Bubq%2FGFgWgbpYaLdf99lbZl9s71tuiUMUXUWziyvxO1uTDVdPTvy0dLdHDjsyE1Oq1iTajz5AWtfrM%2FLrr3GKrxJyE612mCizZJdrNXlRIX9edhCZ619Ru6g5WWcTvfZCRjdtyfc8Os9NFGfKKz4uLSm6GpJ%2BuwmqEvmMRIuXwIXbmBtXg6SdtZBSfoKRxlKP869nSbjU2xvLtkEQBLXc%2BDg1r5VeblFfKAdPf8LkxlSVbPXgmU7Kd3qDKVPypiiWmNY7WuMyonS2ZFbtcaJ8P9CifbSDHR6w8Tt4eUNU%2BApZznh%2F8rkHV0dgZcMmvKMjA6Xr1ZTdPM4A8RQvOdmmCU%2FnU7c3JSgEJ%2FzDUg%2FubqFBlmtyTSEEwyxnN8Bf6ygo3o21S%2Fb%2BfxvWOuVA4owheiKxwY6pgEe77%2FjLOJdkb9%2FMPYbEOIC7mnLW1XIGf0qEgJXRBgsGxVi6s0FcPRgLVfIajeiBtFQTIyAmjNrBXCjMsHYQdOMRU4db6pVvtquf13jH%2BdsGRS%2B9eoBzOevZpzwG3MoAw%2FeETgjyDW9C0aW%2BbCaXQ5w%2F0YCRSazIWt2o3TYqNzk1T3bej9HMWjA6cfTvwy4lujF7WSA0tBUw%2BtxmQ4o2VdypEZTBCGj&X-Amz-Signature=5d7cc9b7c5c8525506c367e59fe425f4d02c7878bd7ef4385c7ac65d754b1560&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

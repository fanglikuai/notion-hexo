---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QI5VXCNC%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQCgN7%2FQjDJLRC8N3OjtWwI%2F8FYqiCfJoJCAOYiIXaQlSQIgH6Hr3isyak2KBo89lIl8dFjTPvux0%2BPzgBFr2t94pkoqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYcOLBVYj9Gf2yn9ircA2dZm90UrS5wIUx5KAjZ9pU%2BeDllQA3XA%2BUpRq7v8XdpZBJgd7fFYR0PBeiW1CYdRmBQjo4ik%2FJw5zz0%2BGNhCHcBC2D%2FfV2Oce%2BMxFL%2BJnFW8pOHHYzCj7Cw3nR9DPu7mCW6uCahvS7i4oknzRDVFlIn8J88A5nbqS94LW2A7k52jkpBgjdoEjhYizTNOU5LBxsp4Eg3QoOqEYC67kY4Sz4l69hki4DJro5IaWo%2F8PNVQCqiPllmqkhugt90zCzQAoki%2B8xbJNAHouTWn3gO8E8YbAzUbkVTBICCmSOFazz33C985Wpm9v%2BEGdzW%2BbMFAutf5y3dcLzAdJ%2FT3MFFEqHlHwQOjX9B%2F6h%2FS88blvMkrEu8gr2zgLcN4BYsu%2FVnIa1eH0IDjyigAcPanpNqDS1m5%2FNCuTYrULTKBUQsHjQlKD4XFaRgV%2Bo%2B%2FDB3NTtCXdR9BTFUQTSnH5c30Hn3GZT80m8gMzznVzApXNoFqPDuD5cNNCSQSLRHpYnv1GFXQxFqO%2FEEFByWRPJfGp1Ma2jRYkoo1jINXR957P78wqr24TW6UEHXGUnAFw1Ia5alo218g54UD3ykGtpPd1EeOD9dx%2B6wDwGEM%2FKP5MsJJrWECKrEu%2BHGqEjwcZaHMO%2F5%2BMgGOqUBT2gPTGHDZfukkuOd2WzzUC0todX8hCJClVFly%2BRFxHC%2BNTFymQT3BelahBKI4BWowqWSJab0ths%2Fj68T48F9xC7xPuxhj%2Bqx%2BFMGvSLQAN1ponUPSvpjUhKJfSt7UEmNhOjT7xJu6MIm5exSH2Kkac8gnLRYIXpUDE3%2BaxnjkxtlllzdS97CDUNovQbmdMpm8%2B%2B1cy6Wh5NUNJuvMYxibayJz5BU&X-Amz-Signature=12a28181a7d6f8d7b8c2076da093a11b26400d73de5658058d14c977307c652a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

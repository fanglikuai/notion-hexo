---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VVNH72DJ%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T160101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCmLGrjLoCb5PAdIyTvRlUwI0qFn7LgFEDL%2BcAjyVQ4mAIhAPZeXVF9nO52bpdvtkDW7z%2BYKVWBTvzomajdDMM8YlyUKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwecammCscfy6VKCqEq3APBgeyv6V5gBjmq1C8OI81uuxtplUzGUakl7gX4D93OwRrqpr8N0oiRPAhQO%2BkVWGgK8EV%2BLtCWyNdU90Zs1hnEN5ltJpiMAMD7n8h9Xlwbq2lB2JsbW5dqw%2B0DS7NTrgAMoE8N150Cw%2FjepC7wnBNdHv0cr4L24hrRTlC9gVnzQCnwfLNnEt24hFzWk2AM9RYF2NfeBib9FrAqJfowuY6Op%2BiooU5lfY5T0KDnKpnLZ5g5Tf5O%2Bk3zpFgkKGzRn79Q2W1z3h%2F1bzmJVTIkVOsGSTkYEfCPZRk6JsUZ9giTzzGGFndbFy8SSG%2BgzDIwHMSbWwt2p06lE94RpQFph10RVj47n%2F9ZZAOTGr7EgcnQSWWcnauNkp41eLIFAZ%2B8c4%2FsXOA%2BYtJ6h0ujuWzoRPPVu3GFWtcK%2FscqVirT87qbWbpjc6V6T900i9Stb13TtRICx%2B4Srac5JsjNGXly2klFjetwAH3KPtjX6VP7tmILzwRTMZ26VDriRxO2M%2F1%2FbrImXk9SQiqyd2BAkQGEOkFZw%2Fe6hNSaT8H4Phgcgaft5WxRPa5QItamur%2FBmLlFSmES3qcNA%2BzuPT6%2ByuIdHaW9%2Bl3JMAIugkwyvT8zsPE4%2Bqg9oVw4G3HK0rA1GDC63ufIBjqkAfz%2BimWGB4AKyaOvlrWnbDp7Jl%2BQCIKwXEMEHsLIN3wKcas%2BrkxkUMYpSzO9D%2BngR%2FzVpiWe%2Bz8Mk%2FfkODDLZTQ%2F%2FXEQhZHP3VIZoSCBru34sXfE4zFM%2BoOMmS%2Fl7eHM6B%2FVJaKk0u2C%2BEpT25aoGK1H8X%2FaNiOWlQeU9F3G9z6LX5SPFkuACAArqm01UiUHJgwf%2FG6rHIdtiTI7KnrQElT7eFdG&X-Amz-Signature=8bf67341449cc9d78661d0c5d932fd7b245fe5aa8913f52450ff34a3e247dd2a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

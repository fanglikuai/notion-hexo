---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY7PHIA6%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJHMEUCICG5XT%2FMlnrxgMwXFpaz4JVsS%2FBWgtJxPM4fyNLXOFRfAiEAiYbuN2xxSU6Ej%2FoPofCq%2FOHfjxxbEVsY3Hq6H0TBHeoq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDPFBHITbV%2BHyts6M1ircA%2Bwis6T1QcR2iv2qKTRfnBjKh5MJord%2B6aNSdl62G2pzIlEKXNb4YYCnCb0gHPZ5GsOQsptcReXw1FfJboPVuMJJFuxKCcQansCAShVu5VcRT%2F8xDrQlw932nkzJICsgM2VluBa6XSH2ETqcp8kvu7Tj6kgZJX9QhD%2Fwgr2CUED8MXCwicfhalNbCst55IG%2FREPpvnerVA3l1xpK6pD20su4VUZnd2si2zPL6pjrgQZRcW0tq8Ej7olb1E%2BEOS3u5MjBUFwrbmHBrBI2utzZqN7iKCStT1yRDFa6Q3FNsMVt3cYXojLp%2F1OBaG8JFpzK%2BEYwhQ7g1Vn9UuQMWoEQKhBOIAAbet00zOMHgJIr9fW7uunRlKqOUgGayYVJ%2FmTZ7AhsZ%2BHNJLvFpo9bmEZab5SpbPBKnP2AcLndw4b9W2CAXS%2FzUeQ2PX6bSpRGhE0vTV7x5U6qUlXbDwc3Cv7sFmZN%2Fqs%2FnxJPsZee8dnxKoO1DIIGm1Q945o4nYBETaVygEiRaWnDQMRRLFrh4YraSK0CGWy6%2Bpa8665JbR1ZpwbtEX5jaaV5HG1C3Q3vC1xpuDX%2FjqZ9Ry4QSFPckl76isHhKf%2BEJi3PZkzL2D%2BPVwxwSYtHAdLV7DfTk2yeMIS0m8gGOqUBKxQGfJvHWibIlOc6s1MLOi3%2Bkv3VGlqa%2F9XhnmG4quUj6LFt28OnkPyeSe3Rzw5uXvU%2BjL36dk1DnG4XTkv%2F6HAQgYJrLj67aZ5NBoPTIrCffI32rcTA1xOk8LQ%2F3cTlpiYbZXyp2zYZSQMGptefMuUXDVWqLH9kuuWcytbJcl8uCUeGJA3ZMJXjNelbhIiXOgLtmrnf5SI%2FmsTCcmiMIdzrcAO5&X-Amz-Signature=a73f94939ec5aac96d36f0bc2516c623c42e348e11bd9102167093f599d4d907&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

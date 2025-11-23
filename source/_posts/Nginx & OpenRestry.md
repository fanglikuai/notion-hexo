---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDUNMV3G%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIFevjg%2F7M1i3Q5niqZS6NZSaAkRHs3qYJvtjPWSGXJRLAiEAklhFipRIFIrMF6uqGEoNjMSkub6mLaPk%2FWx3kcinWv0q%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDDpzPmDdXWGCRGEeXCrcAyRIejwlwUhQoANlEs21AV1Whqlw%2FgE%2B7SW8n1r9LysE6uK%2FJ4KZg1WFC%2BV%2F27MOo8yPO45VLaotoa%2FiHql37edL6xJzVxR5Ok0xGOH2JJk2Dxwn3b6EMZx2lA271kscaghQs%2B4TcpTorlLhudUwSHHFAAqxmKKB823YHJ7RuLn2jPWkkybuZF4sRq74uzlKrMqP6GNTJXTLD9vfKA%2FZK4UgC%2FXc0nBU2ZTGU9preiDNekMsuaWOYY2VgPlRwHd841qMl3MQ9FymXPhNiQLdtBOyl5RNuO9PloxHQsnCWJpxdO%2FKh3x%2BvHgFYIXwdQB4bO0KJ6VI%2FMa9IRr%2Flwlh5afBgqyd6uo%2FSeOr5BcDMRXeGgqIII5lBWcZdZrh%2BkTEU0bEv%2FgYlJH1k%2BkMr1hjhXvnsAKsl47%2F5ybAlJflWt5AYiE2lVCdGxwOBjSNs8ZTuijM0QAKKi47jmQ3Xw9BxETaDv2BKTHzoq11yE7Nc%2B7FwWAtxyynP%2FBBc%2Fp4DCqirHygASTDLc2HiIosmZUsajfAr3zvTViDZHn7EA4b8ZuIggqslCmS9LvB2mpdHc%2BJj7AP1p76SYTEzFe8RK%2FKcQB0aMnETGDyd%2F6sxLaUhd3Azh48ja7smyvgr%2BNgMLSPiskGOqUBGydC9RPiYlGNn4hUUaNw7u7ieq0HpGhEX18GX0u7rLinA2PWCb3gF9vKBUuxVKKpdTVYjsRrUpY9B%2B2QaOWlemPSY6g6DwiAUc25pp7OrvqS0R3G7TJqjzWsj3aL%2BupIFO3D0bTTyvqpa0ajwNAyUIXkp0RkJWAuvEI%2FfyPw8kAVFKfX0aa%2F8HFl49ViqWB1ylJBAbAK%2Fp74mJLJaisVsjcmcWSf&X-Amz-Signature=65483b4edc2659dab1b3f2869b18ac59e87744c3757295db963d8b773bfb032f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665QCJQBWM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSoEbi3tBGg%2BdckLTt5Wc7zFJIq3eBe9pCfgCK9L4gSAIgHA0F0yc8r%2F%2FSG%2B5eBD6Ubvy%2BZRLcEaTe5VbttKRYXyAqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCQlli%2FII11nKUptJSrcA3kr9mGMqDR5lkfFfUV6EcrL0Q5ticHB5%2FbwEyrQJIqk5KvALapevq6nnh3hYrMCSE1cUlQuUc1Qe6zuW8E5N5yGTYp9b2bCJShM2sSpGo%2FnyjvpRIzZN9LGH%2F0jZgebFlI2BFlmqZl1tbiCPW%2Fb63F4V%2BxdsG75bfAamwuzboa1wh55ynFpJ%2BaXiLnzZPL1r0r0tNe7gd2c0oqRuEC1u9QKnYPhS195KNlm4OQDzKFVWSpxSksKD%2F9%2FVZzXUVphyxDZQnsMa6hkdAKHaHXYMc7Mcz6dRJSjuYMuzAZ4A9WGY76zrVU%2BbnkDM%2FVaW5h0Zry%2FsPciQuWYKUDL6nvIKIvT3vabec%2FhkLhvuxjFfQcqQbwk%2Fbk3fY%2BCs0BXHjOcrDABVyhKItk3GgNEkd4ZyNY4U%2BzaLMK%2BfjUR73iCDEb%2BnoVRjck34OmzJZ2vgUHVu5t2aYNZPkQeD512MTKcZTkwsmB5zerY05oV8RVWWQzcCLxLJlqv27581CpecNXIx7HG6xoCpvS84YTnheaGMfYYnftn4nz0FquDg4X44NuKW8KZZ3WhJmtg7rt%2FcgiFU%2F8rhFGk6Vrj3SU8wpz1LEor%2F2GuUIDMyOq72VDjrh51Q2akHI66QQpJCjEfMMn1kMcGOqUBD8O2mNYY2vMfX0baTduRzO3PWBxcfuNXKgyVXiIsOjt89RiRldVxBuLJ2wUspl8dwKWh3p21Gy4oZKKrysNOTjnRpeTt5ENMor%2BqPyWaCWlEys5N4mBsxGZ7kSV4M%2FpI7zdE1vHPZPmwDdjuBwwYhz4Iq2wvhF6mftlO%2Br4HZ8igkJplrcFmUAV%2BCSFPjMGNDpEVqEVyjzb84T%2Fmmo%2FmkocRh3Q2&X-Amz-Signature=ef95ba09feec9f710457905844a34ae0ce7fb09378e254a83a8ad0d29cd4d30b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROD4GWCR%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAl2rZo%2FjstxRd%2F83UhB7Bk5g0VnaDQJtQuac37OINDoAiEAyLCJxXUSYj3Hi9uR8LtibyGNpkMWY%2FB3rPLLBTY7fq4qiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAEDktmlAQ8zb%2FPxWSrcAw7882NXtBta74ZAhz7An%2BrHxIDtvg2AqEqLBJFDicnePLZIEjv4PIvVKdUfEfciSptN4kxOW4%2FeKwJpJ92GpkWam6rXySdFAieeTwwJRjMt2Hv%2FoiCZci0UUg1FZeTBmsWvfyRTZ0ANXDgdcoqSK1iBWzu166zwo8JkuQDomc9EhMf8jPHejHiyyvifWR4QqiIqhZ5iS0vmay6%2FIbKHMEJdOTSat%2BeBFqyEfXDXuvlUb0nkqwgQuxIhF4uQKQAyaYe3F7I04lu%2FyH%2FL3H9fyH2YX%2B2meBLIdsi2Ek9jYQ9nhvYru8ejPsU4%2BLbWlgo6TO3UO8sX2TFT2906qPNaSvJtdCqSRx%2BrRd88e8x8Cfcy8zHRh1BOzkfpLVQP6EwlqbRW2fKcKB%2F%2BDiHGNBSbJg%2BTE1g%2Bzs%2B9jP3zXr0BSnsbYNfDXIecoEraEEopcAo3%2BZaoiXs0jMJVfv2RshNV81HAyYF66nBu9VUCZIdX5BJl4506RlcDa%2FVL0KUBEhvrMZtg%2BCVUv979sCz3cvWzhZLKugxhSw3bT%2Bon0wXeZ%2Bm4SFHgmA4%2B0gfb3HKxiR%2FnB6NK3Q1VlHfyQAoD2pIXgpzU3UqH%2BzqNGJn06m0WNEGH4ro3zZyLU12TmEiSMJCDs8gGOqUBx5%2FNvV65KTqKvVLGgIeZjOYDLGZNBVYXCVrku3JGl5nmWrjz4MLIzG8I0kQUKB%2FQs0fBqejOp4kR%2Bpg23YFcylkoZP6yYMnITlfy5bWu5rrw3uzl1hH3DQx%2B3WdYZKbtAXncXdSQUGu8DmNa2Vk2bEyA7POKAImUWZXxb4ksG4CqVZIqSbocwZBCrpTwBe33zCVMSGTh84JFMJ6qhakbv%2BaiFE68&X-Amz-Signature=21730ef8f007fcb97642313141fdd7f923f2efa36881f24afe7b962fde8a9c39&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

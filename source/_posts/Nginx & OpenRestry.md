---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677UMPNKZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDhRCVosCAeIOe82WBGHltH%2BZCLDA5djryLN%2FgLtfX7NAIgQyLbTpnImb6gwfIuvrARBMqk2HqCJ6YQ9CSP2YpaI9IqiAQIgP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAmtKFy716R7mjBCjircAwpOs6AIRuHnzMkATLNUHkRPZl%2BIeBszhIVQWc72qdBsJtC4LQW5uVpIGXBk690VJug1Nw51jXOnYFhhIL3Rj60fDDRaEPYjFXSpYqM%2Bmijz2jGa0vvpE%2F3mzKoaeumbmW%2BxlGeseYty0VAQ%2B9ItZQNSzGmOsMPgTHSQn9SoXUwEna%2FHE4V5DpqQ0KE5n4UHwMUlE9t25qRq5A2gO2%2B8GyxKPicKW0iV%2F0RzuAmSTbAV1SQXwmip0k8Bg93jEDO9UMOBb7A2w7HphwrMPm8aO%2FuROAviCrQorE%2BQ6YGQhgCFj7evXH4En4mHRJYma%2FmIUJ9vs3IVrhibG6j0YqDoZB%2Bys8F8PR6LcdGBJza%2BoyophaIyRYOAC0GUyBp2Q8OFln%2BR6hM8YbU2%2FL0sfgGUfk3pn1%2FwnWDJzh2l8Va7Kis5M7rrBApHjMUgLl7Yw%2F8%2FN1eQGZFwkFLOVxtzqc2nFE2Ddb6wJAuXzEt7FwWLET6hctE4UsDmFk03WgnZGFkD58vzz5xVLcHltXyDIv%2BlsuwwbzzMvzglRjmWCbEgcJ3b3UllXQIlrbch%2BJ91WfEyLy5tm2skqAdIjLHJzKF71%2BCtZDUExvRkKSF7ICAkYEG%2BwWn2YZqpQyJgR54EMMii4sgGOqUBxn%2FRSdE9gCDTJbK0Fuogd%2BDvp6LMhznh79i9TwvlOVBZtvcXt1sYDsJTzqLB1a0gyTXXDCrvuFa8gqnKNl7hHarWbWP1uZj3Oe1AiqETS1ZAcpFgJmN7JkMtfbczyE%2B%2BqvWqon1HOoVw6aRMErqKiQ5dA6puOBIUPXdhLpJvNimeIxrciL6hnyV4pjLfINhDBSanZZa4RN%2BuB9v6aj7bCvYhuMEu&X-Amz-Signature=9c04279bc8ed0b704f1b72d9d3e0cbc8200a992854c653e1f051f07f124ce155&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

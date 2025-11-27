---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDOUUZTH%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCB8jEHz%2BVKmrYd%2FeYPbS7zKF%2BZc%2BqrqtszoegyB7p4VAIgC%2FRaymRR%2BzGk5J2%2FxgDgujvdxiLV6ixtz%2FxJfJNBrjMqiAQImv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJLFeOlWwtx50h7lzircAxJx7sRgCWM2YyxulzXR16YBCg6HyXNaRrFy865aQbvwv99ps%2FWAwrXZR0GvV6ASwA42zQh58g8WNkLp%2FdzwI%2FBICHzLV801UJeUR1S%2FEqmenkvZxYlu%2FOmRf86aIwfkOl4Lf%2FZHC%2FwzZT1DB5TPpCD6pBdMydDO8%2FjhMyC5w%2FC3imeHQINnRiy7SJ5esF6hsvfHmggfiCSTKWeSv4HX14KB1p5HdWkWo0HbDpDe2a%2F1VRDm4Jnr8Sex68DLX8g2o%2FF4gFSTVSzy8O0z8O42gPBo1%2FdUd5y5lhGIGE%2Fo7yI8I2gPfcZYqlsRWqTa9cOoAgSRQ82yCYPhkT6wF7VddKAiQo38KPRcGrVlW%2BjIdGJdOyrdjGp3Z4IV4lto1C3yJP7lnKyaqo5S4F%2FZCX1TahndC0Ia4OKJThUcNqI%2BaP%2FDNOA67lHjGX6EQiv0BkbpeY3amrBAkekl5qclX4kACe48KASMnc5OTLVzufngdn3Y%2Bdk7exVeWMlUla6X2Azhqj3mQ%2BHpFGAf8ji9SQLi7aKhR6bj4L14Gvg2ki2Q%2FysQchWaMpycUGFaVWO6LzrAn1B4pfS%2FVNBEA4AH93C4VNsF4rdXZuLi4yKehz1ZohGk0Wn%2BgxCaIjrz%2BRcAMOGioMkGOqUBqJt1zm8s7ll6qaORfG3LegVtIiLEy19P6boR6Jp65B00Y09KcY0YIz0BzWaC9OW3FyhSkh06imSyDhDprEx2sPiFRelb4%2FtlSUaoDvv2E2DY2OTM4uTgyureqd5vtU%2FTUCc%2FaNM8SdJeD%2F%2FCeY5yAemBgHbb74lFR%2B6mQIY4JW5MgPFh2ydZtAt%2FWmJv9NUC%2FuXyosfBzOG%2BuHQ66wC7Lpdkpfnx&X-Amz-Signature=5523ed8326551c07566535d1decff7546919f5fa5c9605926f2ee2d8f4352110&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

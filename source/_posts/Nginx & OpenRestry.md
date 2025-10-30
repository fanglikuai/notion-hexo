---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664RWNSSNI%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJIMEYCIQCbwOebCI9lDMbaOM%2BsxF%2BBCuswTvtaDME0rCiM9Sq6gQIhAOc%2FhVNMR6UR0YuCvDSae%2BM%2BQTGnhDGFHc26Jx1fNmdPKogECOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXb5fNBvbQUAMxV2Aq3AOcU4o79PxMWXrB%2BLXO%2Fv5%2FEp2Xrg%2Bi8sBb45cgiyiQhZA9IyKL1GehSc6eD%2F644bR0s%2FveZyE9NbEKRyPfjC8Ou3mpXDx3nzFK66aWYOYbWU9Egm1xnzR3LF6Vsb%2Fh9ftdcEzsXj6Hr7P%2F9DlG9JNvsgQ%2FnHC9ZNBuiRTomNxcITChlFe7eIPDgEOHVi%2BfUsQh7VC1j8KIolaBdul%2F%2F%2FtF3W%2B5rM8yLAaFkqtZ9DP9XDKWOnYHZLFkok7QzJcCltFnEPnCuMuqBufd47saTMqIV3OiEZh4a1OX%2FSHZlZoeGtO8AArXkDVZ1d3unA38sGlp2%2F3FBNBsc3Lf6Y5N0akM3%2Fcb%2Fu1cV840DtLz3zoQ9Kf%2BCYRDEfRuz%2BBYp9R5whwRYILgnLUvfQM67RF2o0kTIju33zBca77UDJTaGgdw6X4TNR35n%2FW5LbtnqAwjbFkzzStQwNaG4gK2S0dsMQNHIZmnQoQ%2BOxxhDlaqinAp%2FpNxtI%2F0oQmslVOOPUWr1GEZp8zIfwHDY9KvsVakXIWDqY1rOmdc8wKAhd7C59X7I1rrO5Dhs%2FVu6LuuyOQASTr8H0EmTL%2FX87LuN3zp7Vro7NBAFvwdTzVKKkWHwsT3%2F2lQY25ZYrRNbylGfzCRlY3IBjqkAT05YUkTSOpimkjp0JErVn5UJz%2Bi23nP3WSKyDdkWobgYeS14BkY1CLf2BfyvWgRyC2YKjDAmjv4VeVJvMqtcgn3yVGfKKbpA%2Fn1R3YYMlqRrBCjnsL3fz5%2Fhi1zGOhjWVnMUYyiveUOQ7HSPk2%2BgbPJ9%2FeKEK8CvR9FUtfWfU9iJeKqV7ycYBs7wg3wKmU2QFlAtgA2hhRHePP3pgtlclVdgy9E&X-Amz-Signature=250fa74724531a390388465359b82c055b5ac14ffbba60c37d9c358702ed0df4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

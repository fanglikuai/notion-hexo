---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QUZBI75S%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH5fwqnEgSPaQpX2bmO7G2CuNHTALKy4gYNC4%2BUexK5UAiEApnSerlQjLkanzSxe7Z7Oz1W2yOmU8Q17J%2F3hD9GEFtQq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDP%2FamuoBrmdYD94tkSrcA21D0mUWe7sT4VYPh0ymZ51PLH7WCtB8J2pAp%2F2mKmx0YtlPAtwSVCZ5zyIyldVKEYgugS0fgOPUUC9QonSUi8SnFy7%2BzpsDXevf5md7vydeOGfaZZfRupaiMQjH5Pd%2BYqxEYh2HePPQgGF3jiFqpBboyr4hJbMDKrs1PAkhLGXY84wkAy65ZZyP%2BnZrjom86C0ZoeJSEOepHS%2FnNbCnw4w%2FgjE1moEb4y2LoOaSdkVEmADF0ffsUM1oF%2B7N3C14XkY8V9CeIjd9IFz33LOnNQ7Bcv%2BqCbTRUZtwzImhbAcJh%2FSHWeTM3uvhHxaPSumS8%2FmcXUtPoIUZNU5WEQSs%2F4apeVAWD3Wfpr%2FYZ7LoNmNzBqt%2Ba8w0tbn97WNHVEtPk7565ljODA1VO6rG2Zlv5%2BdUKFiHFcy7smUEk8kdkgGlFd2ZsQcgDUznc34KsX8k37%2FpSVA553m0nkmSS5FFyMgASM62r8jHDIyrsgFuDjv2DcUHk2WlKFK4epRLvhK1mBGKBMzqXVnOsTZMq4Yb7KtlRh6aP%2FDFS0Bxp%2BFNcMKLXReb%2BrxCFARcYazJdjieEJhyRamms81efXbwNV5foN5B1p20u1mzfcG7GomDe%2FY8M4MgNsyd%2B7jyyM3kMOjnqcgGOqUB096di2v2EFVe5hx%2B5UCpc3BqNzwq7h96IwIOE7bWQxwEXeuptEYkhYY9uIhQtdRbOWD%2FgTM3LLurcL%2F4KsFowi0%2F4sggQYKFyrLIRQK0BUS52cc9LFR%2BW9mfEpOLINOUQSpSiWpiOk3d2hEw%2BB7BeXXtKn5h74D8Nkt2j3ejV1i0ROQNcSfQ7NxCGMOsPf4FzC%2B5tEAW6TZ0QN4y0BZmM73XwbXx&X-Amz-Signature=f5aacc955fd1dad34795e09a869f98158a4203d35545a8ff5a5b7d33e2218858&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

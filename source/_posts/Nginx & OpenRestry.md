---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJS22WSP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T230052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCRakER4Z7M4MhdsfwPr%2FfcoqeOjruEU%2FfqDMJ3r34OHgIgBLwwAgmn6hxPgC8SAvyzkqEMl2yXUcHuA%2B6spnESVNAqiAQI9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMOvtZgwARLIBkvqZCrcA7htZn0Epp1r5JpDXGgvQpcoooRLsU%2BYHsdvNwX6Obi3WVJM%2BYtxp8EihHrJBe4psoo8uBPnWieW3lb%2B78EU2mDgUwuD%2Fv0M2DshBkeTbeOkG7QVwa9HtYbh9JmHdSozA5VhghdKg3rj%2FfBWYJN6XQqgK6dAJ9TEO7y4ockD6XIuw%2BGkKyx3iZS0IFj93V%2F8ZKF98iJqVbptWPIicsLlXPcKs592UcW%2FLPhRFRq2jJe0SmxgE%2FXhyOTAq5%2B3FmigpnUDv2afWz6wpRIWN%2BXhY%2BP9iApkZeNwfMRvDgVtTjQqyG3MpsG1MPohTS6fldjx5Hn3RKaqhN7fPDbgGzn904KHXSubjzbHnquVcPVyy4jlKWs2sGp18l3AsuS0tc1tUhreikzwknv3yW1lx1T17mv5WKy102DbtySyO3NaKWGX6TQQtIjllXAiptYfk694pTXALjSkNcNtsLVo4S6mjA9BKFi3v0i6aZTHvKHTZt1%2BQYld9tnCxr%2B%2B8x2%2Flig%2Fyl3axb60iaNkg2GvJAehp09%2BMQ0tGxaO3UEFJGCjkIDppyigLyAlnjpRq90UDv9ETEpK8lGenEK2Wyphn%2B%2BNsp0Dq8wMS4rbxDYHkZ4Jp4GpAo9nMKIINpeWYeXkMPno2scGOqUBQgxwvl2rbOzirULFmrHTZE76rT1eEvfSnejYoPZ6YosaON%2Fl4kQF2rD3ui4qwbbLYFRVcRAOB4mGUGUoww5%2BKVuG%2BKcr6qhprk8aNyqeVkimFs3TfqaKG%2FeTxRhwzP0vg3sgdmPRHzypkPQZ5ZYD6s2Gpr5%2BKz6LSFsqUoLuoRZiRD%2BFRho3EcGr2ugRInce12gpikubq4x5qVTQJfuD5kXsdiMv&X-Amz-Signature=86ceaf512060b78c9d7b533d0b5d0461fb5d4ebbad3bbe68d1ceb1ab3d49fef6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

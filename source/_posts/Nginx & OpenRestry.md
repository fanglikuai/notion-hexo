---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7P7MIU2%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T180040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQDevQopGW%2FVTyhyPNukNiBBel1A0%2BEmlWbccgSXaiBOaAIhANGCpD3xi2FKXKjeZqqCJykP5cAbK8%2FrQFiJDa3qbLfwKv8DCDIQABoMNjM3NDIzMTgzODA1IgyQVM4ctXgzY7oPx2sq3ANgU%2BiHy5kq3CtuRjKFtYPtcGMX8Vq3e9vuQXGjEa0IFsZiX03REdeWyBbNL01OHrItVW9BbGOsloiZT7Lq3Nf2soiEkIpGlUUtcwbm0N6e2mQ3zIxaIh6KNX%2BvVxgqt3ixxqppIAgdQXZzRfZxudjDnv2OTJDXaQFbKXpxUuJWkLDAEdwLmFQSqbIXxRfM2gkzJbH9KjWwxXxSXOySKDP%2BJp3dBvjAaxhmyvO8lYErWzBtAWo%2FdE3Ch4sOfynS%2FcDRX7hIFgR86Uw%2Fzp3kzaHENywGuZco6w3cZWdeEkpf%2BJ1HeasCAPw1j6M%2BZ5Kmx%2F0Ah%2BHYH7y2oR8MOtQLWVY74XHU9LkkrZFIxapbf57UDFwFRDFAI%2BLP1Kfdk0MScQibp7KFXGTJKjV6LMhn5f6YvHDoacOgbfUA2Y%2BrBo70WSaHVimrb%2FzqIOz5fwYHsq2p9L6D17fb2JHb1RYLt%2BqI7Wy3eg83aTHs9nj7DIeXPXMrd3OyqnQoCExg73ykbYeH0npw1PFPh5qiS0QbgMzochbSkjL7edEPGOjinrKqzwgCsOLWbHmR0X4qX4bAb51Pf2rcRz5XLhzKP9mzeBl%2BgCO%2FMdbYUjvr6s5gr5zN6C%2BuKT1S7FvJ%2Fm%2BkxTDv%2BJjIBjqkAXyfB7%2FOMFj10wqXS6wPPj26SoeIm0cPBrdRdE0Sa6HPpAhxQELjuZ9OyMMTfeImH0kglqlOGfD%2FfGKQpc3xYZscjEpKiTy0AjJoIo8q5gMr15%2FU9VDaUs1iSkzl9vrdpUluqtWB0ue1zPU3wactlipXzKe1QC7676wSe1lZzmMxeCX2uRDkPxbmhYv8%2BfA9HZmpak3ZlV%2FgCgU0ZH5id%2FkG0yiS&X-Amz-Signature=32c96436953b478d9de71433a1133b62c9561f81df7ad8f68f2d38d03ae92970&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WUVMTZ7S%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T220048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH0ZRZaI0Gd97HjsuOaJRRN3UyqfTMudYj9MEIcsMYSoAiEA3tYNRzrDBqUK5Mml8j%2FdzgL7XCbT2%2B1VsZI3kR3gNawq%2FwMIHhAAGgw2Mzc0MjMxODM4MDUiDLXgnJyrpN7vua4vuyrcA4t7ubySPt50NDyn4YoOid%2F9Mi0VkgOP0Iv44R4FhdwvOFg5L9jWcwDinnvjlKmFUVonl3KZN3ycWoHFRB04TTV6D%2B8awi0rHkgsOtmO%2FlkySpV2YnNGcpoq4sPSUyR8qfFkyrG399NMSukWpBrKw1kaOWc6b3YR7YmreUrEJjbiFj%2BCCdFpJg%2FIquMUwT2hFJf15bPrCt%2FzxubrAcQ90MBMezTpHPGIWdW7oIvDbQ%2FamvUp8m5bNSaKCzbgMH7L6%2Fg%2FiRatAAYUwrmi%2Fs1BXYKDgL9iT1uzeWP6MITrmsLTxuejRH2FYGfhk3fcO%2FIO7m21JgILdIow%2B%2BXLpalOok31XSl6WeA4jcI7m4k6uFP%2F60uiwrUSDNKP3chww2ohg6XkumV8JR2JLQR2g5XzZDTCUelgloS4av2h9UkqiweTu0oiXYO%2FqEJhCp0NEoJR2iOg%2B1tikgGs7uaXLcJagscyv0qVdp9kkHAojOYtiKaHCSehP09sQS%2FtGLO07RWETsJq1G%2BBAZkqFH7IUfQYep79z05mGZQXxb4loZmom60mkgmxQxuF7AYniblNkDY1qgHZsny2%2FdCDreRjD%2FvbNajQ2XsHiaYODJfT38DfWPkupfVY4%2FiwvNRS092xMLOr9sYGOqUBny%2B%2FZ4IAkeyZkRLYg6GpXfcvhZx20n3n%2Bct8kjZ7UdXZ%2BNU7silZ3tCgEq9gsOdxF0%2FHecO%2BogsT2fZSb3md7e651qCtMjzjuvPgLSYqMU7%2BDs7KQ5ZIgLCVHUuDwDtzaZN9AeNGBS0E%2FDedJPMGAA%2FVG8%2FXMEGGJbq1atUTAyR5ksPD1%2Bx8mEtaJ4gQWQkaiQdvQn6nm8cPP%2Bhr2i61Wx%2FZmy17&X-Amz-Signature=473e5ebefdcb7fcf6f4537c44cd6198e0e7178daa34f4d2ad3ae55ddac262524&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627UDHBGU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEE%2FIIY9gDHoxGUNTgROBvu4BdKZEeXcYq%2B%2FyPVLPW9VAiATCQsD9PjBTk%2F2VbO1O687byfJLnsA%2FcaSjYY9L6BShyr%2FAwgtEAAaDDYzNzQyMzE4MzgwNSIMYo6uPfeYjrQHMMs7KtwDE%2BIN4516J3tSdr7q3nqKmhyykNiLplufmTapmi7sTgr6l8MWE0iJ4%2Bt1nofaaGTI8m9YojnwUX%2BwkE2ut2NrwEEYsvLjWSY0woqhXdWe%2FWXFFTVIMq1Xlg9PbuXN16AUkVGNB9%2Bclvz%2F7J9defYvp4pF%2FA4AUqELiNefckQcotUv27cSESbpZKddEFQZdqq5kL2kYr%2BC6PIOLpgl6Gl1YYLRd54FMh0lkxk%2BHlJ42POFZ6o8tm7%2FuKvEabuhi%2F2DT%2BkIuXdg%2FdLpKxpQ4ZwxMLVUb4DLqol4XohQwkM56QC3ShTtKP1JfuwGAU6kI9BznIB6QJrZT5dF6Y%2BP9mqBWMc67Zl%2FOpjq%2FMTqjduMHdEmZmWQVUsjY63VeT2DwmyscGQjgwm%2FXcldl206ICLMmTmQ07uHGpQCZdXYqMA7yqvdFG4Hzs2X%2FCPwJ68ddiFA3PoDB0z1B0w7Ectljirz7FQMo%2BzlqW7VcTD0OdjguT0ijwgQ1P%2FDhFLXbgeBqMcA%2BD1uEQgrf8OBetYRRummVMczGpdmGshJBzPQMTBQcHy1V0dBcrM%2FqkppbjY1DDe3jOfjrq%2Bhgio1HTY0p4PTPJrHrvytf37yMnA1LshVqJZaB%2F0p1XVMEYbsz74w%2BLeuxwY6pgHRnDIOMbrClIhcefHjX9jlbg2B5kvBNWwfPGpQnA11JOVwN7n3PAZrEGD%2FP31a9q8BnnCdjZFG1R1v5yIqk3kD5uZmXrYwn%2B5tDlVzd0egRepYCJW%2F70ADabUZms8%2BLAERGxfNS3IlDW8B3qtLd1mHQsRPX7AOEGqrt4AVKyPfm%2BFmzxELS%2FeiG5hg%2Brqf7u5gRDNugfWZf5yf3ajBUAUP8Snl9Bpk&X-Amz-Signature=7c3332012ef02337335f0da8fb2532fae36606009cd7189ebc4fb991124dae8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

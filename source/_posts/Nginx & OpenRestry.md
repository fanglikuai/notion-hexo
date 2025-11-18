---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFKWQK3H%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIF0CJIByZ%2BVEYx9U4DiE3KaOrdZWQkFb2%2F6Nm47Zd3GIAiA80Sa9%2FIhPvuKc7O%2B6W6QNP7V8uzziRGZgU22juu6URiqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5shLJsFXJiGsCoafKtwDOqg2Esd7NSFLu8ebuYgsEdiDA2mzBgAvdcMP4%2BbKfz7yukTqWsTVKe46l9HR1Uy0TDOHNuP2o%2Br2bxjbXffvaeRldMn73QZf%2FSV1OISTcWhbtFnEt3qr7NB00TTH07CqpIcCKrxPD8iAnk%2FzfL2Tpr5ihNE5LrxMZcO5QXIhxmKa7PadSNmlQTxJjMLimUQPbdYM2nG7GREvRMNDmEiin2AXuGLIhBetHxg1O6RETfMPNFKhmyZ%2FtbCKE9afXVkAhNR0d%2FLgevjfHH9lbjPjkOaRc2SvUayVrDvsPju9bbxCl%2B%2BOvf%2FiEZloDCO%2FcPpbrvnkGQjhZpfZ2AA9Fumhhr4JuArDzIAg7pCg671Dtkm%2FkMDGoeMtccTZROp%2FB2cX881iS6o0yK4Ll628UwLVIo0FjbV5WybR5XERoh%2B7nR0yZ85M5amPQiqGAgrjQMe7O87IsPH%2F8QzNu8GLpK3BPSWrPlRfu3aHfIK6wCFNU%2FYaNNYkHhwGFwRaoqFodCFz9dtvLQ6L88s9P9ZDOKOvUtuimmSr9IoCUPY0hV8jSRMK51deNWrOBtVWx3ovcCfpZHtr1UQ6goOa9Y4cRXsP%2FqBGOedu46pDJnuljHFL4TssiF1sHhbFff%2FZYzUww%2BfyyAY6pgElHiPvry0I%2B3MX3l7fhyJZhM%2FbQZ3wWj1JYQ0%2FM58nMM9jZQBG6TwheC%2F0lTRjgrlKPNoWqvadN5j2USp3hjFCmMosJ0D4hgzYx8Qld1oJqCHdHliG8s1HmkpxGgsTN4%2FUkNh8ntvBGrmsCQdtzZSX9qC9OptHmFeKEP0vvob00v2%2F8UBqTDpDL41x3tARM3K%2BQbSBUAK5ewVgZ6r2LpdJEEBJQIon&X-Amz-Signature=debaf81de9a26ee7aed530f9f2d170918a40ed1e2c56d474db553228b84dc222&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

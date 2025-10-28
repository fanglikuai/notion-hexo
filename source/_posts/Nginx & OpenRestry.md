---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664MCMVIJU%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQDHkmGKYRIjN25oP6bQNkvioVBN2lUej0bIcNDOljbaLwIhANZgsiTtce%2FpuvAnAIJfy3FGAZmS1HpFOhTVGKE4DLtmKogECML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgztL2jR798ZpLiLNgEq3AMcUsPYCWwpLJuPU0DFwCR0S%2F0MiIU35gzxnlE%2FsVAx%2BTR3ur15QnGwJ7nsEr55rmrX2LaaXcz%2Bq56%2Baf1ZQTPIztc6NOTow9NOBAuWRLBClcKh9%2Ff83N3yfTAN7Rm827H7G33CbNq77JKQob6WTBzmF9qP7RBFEj1aG1Z7w4UA10apAIHpjktNZHArX%2BqVkB48SFHpbLEDwY72Y5bC4yWk84edbKSropIeQU1Tt66g82FsY9vhdv3lKH5X5knmQ1b9d00Xbba6%2BB5iZ0xjdxZDUOBthdO1GFzQS%2BlVLFiCuHjlRDc1eTOs3d0eRARODfrIlksQGBwFa2KlbkSrJd3TrHow3%2BQo4oSOWtjoZmcm2gsaw9bAngHbTQhHC%2FGu%2Bh8OVNdRWjdrwUuwNMauYPZEuJJ%2FAAUjX4CDryOO32zYyF8Ow05dfY%2F87jDhDKkl0us2aZXfuFHoVxTVV%2BrcShOfhEZqGcjvkerWZyo3NzOKH679KnKESVuVwHUkWZB%2FJsfH8rXx8z5BAF3XMibSfh4EC%2FY13Vfj8f50XIPmlQKOhpljfqNMuRKnNgW%2BRIL8IcPGd3cR%2BKAE5lJg%2FGd7lxUnLFy7O7AoekDmF%2FdHFqhJ0qbMRj8fNul8Za6f1TC39oPIBjqkAQUUV%2B2uz1Kufms%2FPaOLyi6QqCYNRzBDhjQ47rskWd3RxvdZWrvsW3weLTnTltnD5SvJ3jzF5zRBIJcuWvYHTPk4iqGMAJxyqlHTJzxf5Ui5WxiEIJ8rNwcZ%2BDqr4v38NLWUDgW6aKU21xzYzsnHPtZAxBRCPyb5coKFuRKYB70NafLZRKw2lmeAaihmdWu5a5t49oMkaauTkZg8512tqUg2ZU78&X-Amz-Signature=133cafda5ceb3629a368bca6ec62f03aad936e1422135995a53e1b6352e022cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

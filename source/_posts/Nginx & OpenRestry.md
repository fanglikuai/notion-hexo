---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRDGAO%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T130042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9TeMLFtUL0wb9o1Rr2jK7jhqnKj00i7X7fVzQitBisAiBbaysljLw3zi711uac73HrBsqECXSaxyDwiEsMRCKRTyr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMz21BkyV06jr24tcmKtwDXARca1I8lCwSq01Wip82jup79xmAWimnd1FEuoBTB0NvvjpzyuSg8w%2BJN3vYRHZhgB7JkAikp5cEGFg5mcQi4lxRxCCKi1wODRQjXl3VHO39QHpMbfN1sArmLrkiULh4lZ1OAvJFwpwt%2BcWqijziFe6sGtLECNz2FSXtZ82qFrEIRsoh89Bf16rKF%2FrOB25SU8aCv1U%2FZZp4CSWV5UWHzDklw7nlK2%2FEqBekCVuf6y9DlnEBEHFBs6%2BMAPb8%2B0ZJxoZmSQVm%2Bof9Tr1vX%2BP2qLDFFk%2BqWAAhRZNGSP2HEBAlqicPWAdic%2F7KO7dkUjFwevZsxKv7unQ%2Fu5dGUxJxPiRIDUESQldAUt8Ef32llnDXKm6LQR7MD%2FGQ47hgU57G4nzQMHEfwXtAUZEYPTs6FdSJ8Pr9PiR36s%2F%2BsAKhTpO2lIwPJ3GTdBQhCWa0ZgXsqgi%2FKD9gawREeMYc6UcQFY56mSPm%2B%2FVqHpkUs4pAZPxDS5CNnK6DxzZv6GtD0zz1c1kd5fiTbZs%2BInu6D9tYTXYo27jw%2BCIm8HDodYxNdIdtUftvtUxE%2Bsr70Hkhf06LpV09Z6zCxiDXrdJBzb2IrLQ96OfRWWXjSVKBfguv%2FWIn8ciXBWYjet6LmWYwvqKJxwY6pgEOeyg5q6g1XyXl7sf%2FK7K6hLhMVV9FkMBScbALZa4F70I20rY6GloyV%2Fqp3iS0kFpJyCU5DZGStweZblaKQ9cEB8BnOT9eRg%2FyRR34c%2F8Bqju6LReXZRiCspW7RXjw%2BHwxZsjMsfazL6omZgigGmpyeOIMWy2Qc5pJ6%2BixHUg8Dk39t7fevoYktgAugXpXiIDZpIFHH7S6HFoaNuGq9G1vkKwGjHrI&X-Amz-Signature=724b86d21c3c63af81b27b5b25dc6bae8a043792dc7cb780655a328a3f8a0c5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

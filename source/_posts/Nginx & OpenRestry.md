---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQBYYAUJ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090019Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDz05JrteQzegC%2BUErK4rMSRdDg4ZAmBnpYiH3xMIA83AiAbgwjhhcnnCk%2Bh1%2FOrC06LKYeBHsNpIOGmU8F4QwjsrSr%2FAwhxEAAaDDYzNzQyMzE4MzgwNSIMqn1cPql3IDM%2FjiAfKtwDFNhv5R8y54JqkoyjCd8woluhauSafSVrtzFVc5iGV9hP7bQ2Dlz7huPSa0aSaZ9wsvHM3ybGqzDhKrgNmOmvQHzm68LYaAVRq4B%2BqQS8jjNwwhQPudH1WNOya0rrhQMfW%2FLYvhW14dJUzSC09GCFdFJ4RpGO3q43NjLsFB01b02i%2FIXG3dYj1NM6%2BgPKp%2Fe0Dm%2Bmi0kUiNRdopDFmKysLjrUn1U2DDWxmf88XVE%2Ft6%2F8BGgaJdSo4lFOrWWbjfl5MvCQyxj9wqAb6g8v1k1sHgagldSE2YlnkQEpGUrSLfutwgub7NSa2l3Rzf04R1pfjEHHGA0TeW7%2B53irkoscbN4R8ryxgX3%2FpuRsEThl0uxP02kmxGTfUnjNludMbY%2B84Uqe6UZ64nisWxwkoGBWXdiy8kecfKLzym5KI%2Bf9NR6P6WZZJCPwfXqdYG3vCSzAgpyj7d9CQa0j5ZfzrRRbQV%2BEubVgO77THg3vLQcodrp86%2Fdh5n0cBa59lmEUQOhHzhSdEqwB6y70UnsiQ1Dljfwj6W%2F%2FKVKivOyt0BgAJDMBHBa9AnZmfmqhp3YWd%2BGGRtw6q5gA2wXtCQgOMBaLPPemcfKRqqCrZO5MWqpzot6s2hdTU5yFRuO2Ixgw7pOfxgY6pgGPn9p7MEkNZ6C8WKs%2BeU%2FOBRhuU16MSgIDWfAE9er%2B3iH9KQSIyIAsnpfsdi5%2BDcYA8XitTgV%2BfV%2B5DEykffk5DR%2BHqiED9LSpmJeg90mnfaYQNow%2BPCYL5plu1c745eQ3gAyN%2F0aE6mYxe5jikoS62lHoQ2Og9sbXg%2F98e9E0DXlm%2BFrxQOSesw05nq2Ns2SPy7ycmq7A7SfrwRjsuaz%2B5IDd%2Ft2e&X-Amz-Signature=7d78b8bdb4de9dd4b8794c9d784b129774efd8b3c46d548fdd065408eb3b1e48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

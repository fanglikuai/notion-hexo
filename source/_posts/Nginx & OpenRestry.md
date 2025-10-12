---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RG2BE5NY%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T030045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIEDz5g53aE3aFZMHZ5ox%2FCqppoN%2ByBIH7HuGaFWepUHdAiEAw4iUBBmaOBZlWwdyRErQoCED%2Bj2I966drJ2YA8w4ByYq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDKGo721q2pJjZxjHJircAw1Q5MCfLFKC7fpb8WjUNDE8zMrr%2FxASYkuLNF4bpdrBUOmoljCKZMHCcFHNmETy%2Ff45RtA4LglVpwzfRyybeZvQAeFUOUDh8WbH3LUm9dL%2F5S0GDmfOWv62yfGvRMmy8x3FZe%2FzrJfitrMWQDAUbAfMPK%2B9KR4cT%2FR2EI2itrQ7oPMI43rCpwfR6X068LMhf1gfYOPTsEVSunAE4u3ErCxkn%2FX8EhaGkfKh%2FDMrDkK2Ydek531OCJt%2FTqhbjGzhp3nQx%2FFCqRdaWw4Rm5iV6f0fovQiT3M4mKnkjO7eRe9NsplPbUOlL459tyNT0EZ7c8TFTgE7FTDEszJ%2Bc8n4%2FvgzQ7JwVbI83yPQRISZkjB4Kz%2FQpHwNDwmFWKvfAFRTinuCI92SM3ByIh675gr%2BM%2F1%2FhAe1yZCfNQu20Hfs%2F9Ly%2B1xllGg1SPgXVfnvoTKS6scPA5j1K4KLv1HmHJKD8w5SLaucKuMMLooX5ohbnXtcmDLigdA8dCdf6bc7wMYSHtjLY8rRuC%2BZ5v06G1eJFG5D%2FJOtoLXz8I81nq9lx%2FRn2Rb8uFcnj5yOwF%2BWqRjZGxnKVM%2BJ4B1NVGVbZWzXTKb4qpihColug9Bno10xlKiUTifzthVY9D59O76LMPOmq8cGOqUBMo195yrEZsbDcYh8nH7JBPusZ1Td1xDvM%2Fpjow24ZKobdj75H7WfFEGk2qIJc49m6n5MqARYc0%2B1nMj5haEAdEnhSEupU6jsz1LDXHguAxVcVpRPi5gXbRs4mLU2D9bnS097rEqQAC4Rr27ZVd28Ow2WvYP6hGWMhJCmdsJvWwolW%2FYM4AX%2FOruDKTmTXPIeG9Tt%2B2umQKbBhNNi1qk8secF7tw1&X-Amz-Signature=f4788c6c035f60c5b167177a3cf4c4c11e52504f6b4be40bc36f774723a48307&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

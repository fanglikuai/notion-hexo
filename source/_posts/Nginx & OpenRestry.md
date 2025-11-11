---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGKB4ISI%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T090044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJGMEQCIDAxWNiHJwQvWmpskfhdwSQLrOLim%2FxOOAv8luLMZo75AiAyZ8kQQJGPfxJzVAxg8beC4doWi7%2Bkv1F0WwGGj%2F1H5ir%2FAwgZEAAaDDYzNzQyMzE4MzgwNSIMi4ftxkzirSOH6LlBKtwDZhuqB5ARoh9y4e1SvbgJjBXHfU7Evjcf73Pr%2BBqaTRifrwAHxW7qyvprmS4Ctz3ogVUSQw4bdvYwjFk7g4fs96ejeDl%2FMLW5BcnZr%2FyLkW0emFTJB3ijb4xfzJdBueGSy5zqkbSX0B8J7Ls8dZV7jP55%2BrQw0BC0%2BcAWTdgExCMnQh9n0G%2FX0T1kZ%2F6a0WAubrSwA2ED7hSQxStpMRRzcCeD%2B%2FT%2BiCSkUCYsHDaNfsk0xYz5dRP54fDyFC70rrGB3mxNV42sxgIvsM5BUzIbKO4gN5WWHZ2CZaSHZSsHArCf6O6A%2BfjjOGYxS4M7sPc%2BYAY4CESVj4O0jC1CRZIGdpHRd7feGIgu10VLWXZWhfbosrj2ej%2BvQById29dDZPWlkFHS4nzuwR%2FE2KdfSny8HPjAtgTiPQ8Nob%2BXHNeDIRcMoUN%2BT%2FGM8EuanzCdHcHKo4i5JR%2F9%2BMRtJBbTkFewTXZNZnF%2BtPoW%2BPL3xIEV5WWbMU8yzzMuTBWD6nvx3339A4PgAS0vN442Uvrw2VEzR6p5eFfzCK8NMpVQXGQEVT7ZNVqlej3mNvddnxeNGYFtvvbWxjwxmLgI6Hw8dnNil2oZUx2oAZDWuM6KNatRFUIa8l8%2FfNL%2FLeA3ysw393LyAY6pgHTrG3zCPv7FQapdec1%2FpdIjczr6%2BqpGfedLUnPbtjEa88BYJV9qqA1xx7vfYhirHvlQHPRVM0xmCMameL1mapdnmIugCdCC22R0T5tp7miRJZz%2BUlIS1bPQ5ub%2Fs9F2LWr%2FRBmAVEFJVfjjUAYWAysWe37jOj28xaNnl%2F9l%2B9UluXI4ie6ZGDKE%2BZFHl%2B9YCn2Fi2SazhC7ZyfuTeuRpYQedRJeLcD&X-Amz-Signature=17f3c739e3397273cf7f194a28d270ead6b027cc34e68bfd779d02f6e4d16a92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

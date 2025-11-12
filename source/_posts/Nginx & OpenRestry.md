---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46646DY47KP%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T140039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDRkgrzk8JV5lJ5MFfSzQ30Q4nKax%2BK57YdcsAArm4FyAIgfN7jBY6sFV7TuiaMGTcZ9RJhCw73cZ1QiE6Gcf0A%2BDkq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDH7CQ7rv9fyZLy7oDyrcA%2BJvbZ2%2F61aEWZx34%2F1qvIAphOXSW1B0p5dN%2FDH7d1WlpD5WWz7LiYcoCvswQkCY4pnhjMkMVn5VotUP9jsvWpfY3nsOYFbS1aP%2B6my8gLRs3d2zpLqH1XlW6i%2BX3JwR0gK%2F0LwLVz3ghPH%2BDMV1KylGfFXPrmjZHsMrFF34WsM5Rmoxb2k2BNjtVQCE%2FvKkGQL6UtiIlCU%2FmZksqlSMp%2By%2FR6SRGSON%2FU%2F9vA5g5DiHCg%2BjOf6YGeZl9dyG%2F%2FK6ck0l71yu0UABaJCnOc%2FEeGahjCvkwD%2FR0Bwl7dqBHlwLkDDvTMyv7i%2F26DDvfmyOG7atLRSeVCKzI%2F1M1z%2F%2F%2BqY24zD5wg0XWV20Hpyi9kK7mWX6puB2Z2T8oOQUo3M%2F0PF8oKG1YvpaksvPo1Jv6om%2BdlFIQDUkjETHHahPN30qFyKKJLcreFdbThnaOcgtXNjWfMev3uxUHG0UQaySsySDwEUW7p2dpP%2F%2FEimZYbJS6j6DmJlEX5bl%2BeVRWF3%2FjaS0rQd2e9d5laDNxCR97hU%2F9oElKckD1z%2FXztyuU1Arog3tNVWghJhb%2BXm%2FvHssqKYLqe1QKm%2BB9YxbbnqrhSGkOfb1j1SwUt9Lcmryn8HE6D53Mbl4ouiNODgmMIeC0sgGOqUBYEu1ErMclZO4ZDm4%2BEAb%2FUnQawlc3MrkF25%2FqQmN6SgFYbLGIX96bG2PaSuSfKyFF1uhJdTTM1%2FxA6xIvH6a1HCOTnE8ZZp%2FM%2FEdMExUdCc4KHNSAxGt6S5hX6rdand3pvOHx2W5aO2rUmiF7JBpNE8xZXi406z1Ob4rTEtJvsMEwTe%2BqhZ2mJOOnFdr5USOOoAxgHhLjqyFjfa6uYiZmQI3GV2H&X-Amz-Signature=4dc00e9e8335e3f1442456b20ead7a63d9294d80e5debccdd6ba30d6c2e9adb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672DOM3S2%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T030058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDe9uZR62RX%2BYz3pS3kzbTwCH9gP07V7MZeDWrxyEM1DgIgYMNvecYNC%2F2J64BawkAuHfhi4k1zzznks458dW7Ehdwq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDAzaFTkDV35M4jCLMircA9Wkdk%2BWAeIxpKNQHyeo7HG945mHlGI2NAbh9z8Gy5uGldiFTbB7Db71mXfC2xCZFQzf3%2BfcJpEBH6ReEJR4ed15DQP%2BYVlbLwzr5qfpsFiwCchLf5DBIlQQzuL8iiLntgM2Q6yvspR4ctSJGGvsEGwyClkXriCMKTDXSPLYLBchvbpWtEYrqHzhNknz5vyhjnaHrA%2FL3ZJw5TjYsBFmbE%2BwoRCeXNjAOUSFysjBLZ34At4I5ly%2B9wc4k6tfcx9y%2Fwo0DsHhkwpp%2FvLWSMOTJNJXfK1Cp%2B3Rk8bd7Yh%2Fc54wfjZjDk3GfoiALrpgsqsPm1jAuH321gC7WOju8COFiZCEWkSSsv6wNQR%2Fkju8XEYJRLWe4HlTMfi2Ic2X5TdZYwEXlDlteidR4qI8CHArP4WQ61K4nGBFxD4nmfvFeXKVbDu%2BMm9ss4DI%2BcApRin6BgyF1BWTq1TNWaXw321n0jagMyiNFroKT1ih7XBufciKVxXvqzlRAcEAasclwGZvCUypbvfKMUCvCda5Nnrh2Xcp57i78oOd1cYzGPEyHaOUbuOITgaBfnnbMwuvJ2ZB0X%2BQY%2FkdirankmOAPNDZZc5uR%2FjUdyC%2Bo59WW%2FBZ3nuYLcob5o6r9NLx5TpOMPyd5scGOqUBARX4KiLLFuEiiHOjX9D0KGSAVt4no9LzmjZE%2BPUjI5RzFjKUT%2F6670ql1ucCQh58P3z4%2FiWxX%2BdbYV8xEBOHwjzVRY8Gi6hl517EiTHhCmXHTIZOBAKqu3wGXnA%2FlgVomh3%2BCHTcBfNU5dRzNXjpeoJFUJtgdLs2VFn5h6qhfeBxE1QYlKWuNUVleWDpkiELBEVTkocdFCQvNRPhDBWPR6hAGT0D&X-Amz-Signature=b78c1f7a3208d144dcb0de28b489ce1f21d67511d40951930a68d114eddc1851&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

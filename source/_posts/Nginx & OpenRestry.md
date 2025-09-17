---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46645YQRRM6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T040038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECQaCXVzLXdlc3QtMiJHMEUCIG3k6AROPt6wJyjn%2BeEkBo3qhiHZRgrhu%2FGRKc6meXKqAiEAhNar0DiODfMhQ3FTCZSDkDG36ktfE3XJlGFNwEQQzBcqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJspIDfRBV5Ls%2BlM8SrcA9PyL5L6FsbGw1GU6ezLbJm8SIfwqQppAuDGRTpLxRWj%2FRYnaY16vR0G0knTtxoNRLfhKvOh1N42HTtjBTfY0zAVH8g%2FAZJruCg993YG9GV6skCqPegyHLcDDu%2F%2FpErE10WCz2voeHVN%2Fq1XnlTHeJwRLeM44kbECf2WuJ5p3BsBvLmFgkQQpEYjD7Vor83GmA8r0Hx6IX7GQDzawvg9Qd95Av8lA2tanQWccEggMFOsApcP1KruG31VLcFm0KbdKjNQ%2BXb%2FjdTMjYZsKMD3rMl3IRxgZ%2F6wPrAvwMd1M0RMUFN%2BDGhhn63smz9cnqjAKlp3fGRaDLs2TbV6kan3W2hbuVm92E7Ci9HxZBuYrqivvO0xlBVvZnbxEiMOd2Kv4pWabCmfeMWXKM6i%2FaRaR%2FTVeN2ZK3KwhQBiiU4DwzU4mjGVdRkNC6%2FVmrj57IkGr9p9RWQDrh3%2BWIP6WMS6DBzfD617xgQvGnM8W0OVVSD47rHuYRKX%2F8KeSJnizYs6RgzucB0SM0XVOvhq3CFBTluQYP50KOonvS0xwAfegL8XlcrL35RkYpll9BUtw006AwKXvsaQet6vpD6EOKdkfH55BYoyPhZBeVUeYnxBY1Qa5toRfceG5V37gGuvMKTSqMYGOqUBi8CW%2F9mP1omrX1vXnnd1WKq1JMik6OhCY22fUGyydP8I%2BTMJod3BwldW6A4xJec4P2Dh%2Bcy0effE1fOSnXX%2B%2FdB4ibcnHgHjmak9CipKqOu1kFw18gARXyeh97dDmPSrckEKGf20ctCf6gbKClJeSlhk4yyw6PBYVXNzeH4fo2dqMbxvjpERdHBcBLf5Eb2reGZtIIHcdKEaOqDspsVA58VToAT6&X-Amz-Signature=b2d5de915beb8d8e90b5d72fdc4ad2fbebbd84a5847ee3d5045e4f496e9ceb58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

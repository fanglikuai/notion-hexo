---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TH5ROVTL%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCICSgz8mylNQv%2BKA0rzRg2x3UCqGkz2R9Zq%2BCZVhL04wRAiEAn%2FOy8lNibW8UkQ1zO6zmsGCRhhbMd6hi5bbXS6gMF90qiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGngDjpED1SK3konYyrcA65J4lO70%2Ftk33wXq1zR0%2BWJLTMyb0RAInNfNnaEf576hXjFb2MTtL0q%2B66SBMZIVXC%2B%2FeNlQhd%2FQButZUxSoZPxmHqXbtGDOG3jPzQP7dTLsuHJEp8Ht%2FHNVSX9rht4zgBH7diixfA%2FqXREBXftE7FvnO%2BM%2FxgobN6GtumMQfr46Gdlvxp9xVGaOatRXYxXArK1K7cUvq%2BuXyZqSFgKbqcmoh04kPtDFOwkGmKSHdneY27pCBr8VAvvg8FfznUZVp4FqhAEIXAVpmWa5pkbdNY6rpptUhOYxaicrxa3daMROspXQJ6ukLtUmgumnlvWB5gQVUXSdpeawfLxvhf75LRM%2BRen6MhywmocImj3L2RBbWrtwP23FAmORQ6rOdWiy3wgoSf%2Bt9pNepsA2vDq3DgqT098ADHLxqomEvbXr9V6s9ZOkmtT2lzaN45Wvwhd%2BndIdU%2FGJDI5mn5rVCHAai%2B%2BG8Sl%2BNCm%2FXtEF4LxFnFcuynoLHclJB3R%2FJuqmZwT0qYOZM5WXKVxx03QnCoF4mFInNJ3aNmv7MjSX0%2B6jBWiJscMq1L1BUrAHWfcRGBw7%2FTDEYeTNUNSPVcTbt6xHn2AvuXRrxY%2FxuXZC7mUXO2us05i1q4Yv3%2BQT1D8MJfX6cYGOqUBmJKCtOETd9CjqJIyQ1sMM4CAOU54ZU5Fy%2FFkwEhq8r1i0dIk%2BWZcK1mhCWaog%2BZ49xf%2BA7ZeN%2BR3tgahNL1hV09Nn%2FZoo%2BJRt%2Fi14XZ1yUcnNq3sfRnRG%2F%2BMxD0vB44x1QGQicImlZwD04pUiTy739CKfpg7FptYWlK%2FHgd9g%2B3cN1E22BIlU2QfcPb02K1JA66y%2BHjybR46HaHeVG1VNwhNH21x&X-Amz-Signature=aa03e240e7538e0cc607f2d212cb16e873e1cb28d7ec8744b9035979a421597a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

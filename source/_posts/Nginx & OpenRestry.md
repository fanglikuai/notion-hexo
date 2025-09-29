---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IWAYUKE%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIGOcwqyEN9fKtLShdf%2F3cBH0sFpJqr7tTkfQc7M6ZHLcAiEA8epYZreLvCQT3dUtZcpo88hmIWFD3lRLUcn6O9WKwRQqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKJmb4IXsKTAGfjx7CrcA16ch0lRumcAjIEAIcD9pGqufWcx9KkewMXtuoj82yEDOAxCE6N5kZYZdtRZFWMlOGSAUTOlndg%2FH22S8%2FQEw%2FDaU3pwsRyTykG9YIXvq3qZixQLt%2Bt5LilBG10%2Bq4pDswOytFX%2FCcaEcbasJW3YanhgRVemxrRDUwA4IyjruMWGp%2B4JtkraFLx4fnGSJFDhPCX3q%2BUjmW9RfoOwOWv5YCgDHDfCo1jjutkyuMi2jOesxlL8fleIO3b5e9uv8gzTjtx1%2FN%2FTkBIa%2BfZH%2FXK6XAa9cTRcXsTNNqasPzIlVKaIAVIUXJQKZw12aIYT1LJ9FqOsJOvc1lPfO2AJ0%2FkzyphjzAKn8VRICYXXq2mpT2L2ymyU%2B1B03F18UgT%2FrFxKB3qq3IvmKcvMPcRLrr5jsbxELTFM0qKNFdpceUCHiTYXT%2Fc0N0NrWZZYz3F9rBpEyIdSweyi%2BtrSqSuLWQ%2B5%2BB3tt75MQfrZpsk6U6WrxvSX9112uCUqIM4dWuWuxRLMLL324SGpG6bgf3wGIXZNVNIXjSRuz6JUKRPL%2FU35fzu8cwizqhD4ARVbo6sJ%2BWhMmyG1mFYEAT1nt42ynTFnEd3%2FBX%2FqknuvMngNu8lXlG%2BBDgZfPnAVXeuiTPL2ML2r58YGOqUB0SRm4r2pvu8svgVV9YDufwagpF4xMADAGtg4AYHd8jERAWBS70DcXIIhKTjs2ZH23Hwvw%2FcN3WCxkpvnXC0HgujbueFZojGCqC4DraLgEebSvFF6YOALEXSRVeIIMD3bVl8PiLVfnvBCrOfmQ5J8bZyASoRtpmE4gcHOQm%2B1rqOrDJ%2FWOKQXa%2BimtfSkm7JhnzfMO%2F7DzGGT3NLohb9lesiP17X6&X-Amz-Signature=37a0553ca9524e26f65f8bbb5f151bbf4f729b47a3b6717faa68a3a5a14b90df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667B54U6GN%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIEwZdC1d8oSdDak9ZaZ0LgHndlqY4p%2BFJPlTHIX0QtlJAiEAxRUGZLUapYhxsUrQaKKsT8ACgmK5eOIYHC26eQBhPRkq%2FwMIAhAAGgw2Mzc0MjMxODM4MDUiDDyxxu5Kf5lJ0TDuhCrcA%2F3fEISQ%2BRPQCyD0i2Z2akcyUncr2Vr5Kz3yWhYHMqYKxNVWmYkEbnR61uPu7myatfi6sTzJBASkzDv8ZcovNPmDsuX9mD2FUvwSrMi8NL2CMy1YqzMFbfwP2LKTyJQiDQ9qqNfR68Xpu4PwnsEKiqMYwCoSZDIpwgNWiPu4aqOIvJsCLuIXwPzk1VWNWGOBMRVxg9nDwTdaXDFIRTX2pnzGvX0floKa9o0qsckOI8qNjh0G9YwqwfDUqrDkvLj55M1WguKKtQ5MXe5flsERmFpc5aNJN50RKwJhpEiJLX4ymcwFf2uzy1XdzbOSF4i61zcF2FZCKnGmag8YXMTDZw%2BdixXQk%2B%2BQx9DC7TC8h31bKgSw1FWyj44OmMhQAMeGVR%2BWIoTDAx%2BOWMfy1AbzD9AqwvJvAf5Wpyl5YsypNN8CZmW0za8TIi%2FQANIxs6QnIjZrqYM3qh4xoY8bD3e9YfAFseodE05PvGUUAScOra6iSC%2Fc%2F9uxg4G%2BLArv7TraTsqSjcBv9%2BHHHN64RNgo9sUl4TL4S%2BFRdhV7sUpMmY%2F2bMNhWkF2iuTvEgm50oTBXnGBJbE3lNmNZNUQN9ZVOnG10KoA%2Bjoi7%2BCgq18c%2BZVOXQG2ZoHU0uwfoJTDMMPp%2FsgGOqUBiPBV8F0NboaX3SnDcJnkvkQn0YpxHy9f9k9f3cjLsKEubKhVwhdaE4hJNknH7vwSSlHUkh4YPH%2Fgq1BeHIbPBYzwsGVSLq3LUd4V78a6FsR0DnpDdWbfjJBRv1fVtCGXGZK0HUa%2BeJhwWP3nH%2BDjYyOrnyyEz7XWqtWSZO11xJwkRj9ZCKml7CDs1rVWpjASui2PWslbXUDF2q6Im11ImDuWiJTK&X-Amz-Signature=96710ae2746a14bb022c83a114744a46c93743b0ab3a51fdf061297a9f921f16&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

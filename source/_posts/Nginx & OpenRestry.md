---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VQ7EV2IN%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T010043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCIAh%2Fmxhk2uit5ykJd44CHTPZ93XCup0KH1zD0Gf2RxePAiBbQqrl0VhNfSz5q3pooVljfmcFpx2ebBtlvvveCGkclir%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMf8KPq5BT86H3eF%2FKKtwDglOPJM1QjZuvwgVthhfM4iWCdrAs7xgGtsV%2BzMMHdWPI8thNeg3dQ88gX7Sd2EaMkSskRtXfbYwpEp2D80w2JZSjGkN7VY3JDAFd%2BlRnuOmJ0hTu9odBQkp2XLfJIH4VuhJ6YOTyXxg%2B3HpBT4LPAb279I2vTKx3u5HTBY7BdOCCugzb3osuzlBv946ECXVBMyqDQfuzs5%2F3c5FxDbvq1fsel6adBrBsjKN99BmSFQRWa358O%2BmideB1Fa6XE2lmJnY%2B8uCM0rBtTLYByXEL3NI6NFR05p2Qeo1whcWZ%2FhaKI5x91%2FUpYmEUzJyx9LzRJ1UhP45MSImgMezXKViPhhlIyenvilX7g8MnDuEFT3P1Hpwx49LHUWnSCjNMX26qBlwnCx9ydlBhBBT%2FTuMP0q4YBs%2BEG0T9W1cGPiUG5wdHJLxi82WvqvzND0jFu5wrqHbj4zOz5L%2BgXsflOnvAUBFCtB%2BVrXnmqEHHB6iYt2Gqm%2BQn63kCGNWN1fPQZ2e%2F9PF6BixzrfrJsKgzB%2FJvS%2FY%2BkAqdmoKgrkDlUHH8RbzINE7rwiUwyjnfPzNZ2CyL7u%2BiCCLXZHFvWZd8ovElrxIubFy%2Fioafh51C1itZ%2FyxNynjkrB4SAdbeLo0wzo2EyQY6pgGb3uMxYf6K7b1OL5foOMKlhddlkPEzM6epnQinY1gtuD8gn3mx1%2Fuz6Y00aw0UnPPArhnGQPv1vApeDAQTSZE2shK3EdIXzYUrqab5WXSv3PC1hf4LanAeCtSdMKGf0gpV%2BESLUA6e%2FVPTOYr78ih8wUqLKGza8C8yJSwjQqgaj6075nu7aTxPI7DWmxPNulkBsPkKpHwUsJixu%2FlxxQUYdFGpqSEO&X-Amz-Signature=28a9a3279b0d58f3b246c53116bb0f7d77bfd146a27b0b9477eac0e2d2af7e7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

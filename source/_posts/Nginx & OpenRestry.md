---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDJASEAF%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIFYobWmFcLSjv%2B5m42HGoOjG1yeDhN%2FXMwXAUtmM7q7dAiBI19snlBthXMiux5GE6KntZ84MlLikeyc8hGKJar9VhSqIBAi7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQCAOpQrkiN91PtAvKtwDzSvVA%2Bd77yCjU0xPCeN03FMLxTh44Grll1AaClnMu41sAUWWOXkGt7oFVyTMPFaR6JDVJr%2BURQZdJ5h5t4wKGwmJqtwr1f4KBX627RONg8WBdu3f%2FtlLRMKQtaSMVRKaXYqGu36ddiWOg%2BRj1wu110zqVIgMBn8XDjXO6M50an5ScBvFid7L8YBU3ehBZN5nEoWk82Q7OC39ZW8QfctuEe8q66Uewi2dC1MWRgjVTyZm7E9DCS9f6%2FA0g6j1wxXxSlr%2Ft78OlRBVWXeqW502GlmbuZWYIQGig45iIg4oS0dfy%2Bddd1IlyGQhjlbq7inh3YaEwiK9FmV7Vr6eT93A6MLg3TaHMSMrJV7cC3EXr1uoH6EY%2FkpwcCnLUnvOF3JYUaEBZx64lQlaoc%2BO%2BsX3Odzh0qSOY6gl2716iLwG4O8xhrgWDvlxGZCbGFAV8o%2BBzL0rztNqtWPJt4fmjCpC%2FNs3a9m4gV38OcQ3ad005vPADm2V2p0Vk4Q4A5Ev8IXLXkNRVztgqArL79VwNzqkldUFm6C4GqsgsjnrgTxfWcLoRsNegkcWEjI3wGls8Dp4AThDI5Alj9yG1eBXTODYzI11y8RcZzi3JddPsPxSrwX3roRAKHxV9b2UDCMww47kxgY6pgEjAi3kniMHYEHVc4%2BnLz28isSux9pWH8LwHJlVjnjPjlwwsXRNPBe32rbsSezPub75TyprtN3fBIrCGnO%2FyXY26ag2nYfHpQ4Ti9gM%2BfXhFFmMkVRLF2kXDxzsrlgieCFtW9cb7%2BOZGZvWIouFcPDWH7QNbYp34NN19Cl%2FlkfU4tDIn5Dzt1oUEHSQpT4gTghHWcVd5fuRo3%2FU2goTVh5lwDDLHLx1&X-Amz-Signature=71268b7008da0d252859e645eb0d72b1837f92cfa0df5a4fa57428ae72866a43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

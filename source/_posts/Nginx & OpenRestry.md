---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKH56EXX%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIAiSxzsK%2FGhkO3oha3aI0giaptCG0AcexDXiCPS8LY8XAiEA%2B6nx4MGGugSsV%2BP1YRkQhARTE9MCwL2eoDjG62PhNr4qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDItIzoIT1wP1yNJzFyrcAywu1%2FfjFyEsk8EpwnBJe19FUZQEVoaxsJXOFoy5Yn2ONBw0GgR1wh909kqf5WJH40YUwISGsttioBhOql2nTtevHMXY0xafhRCrWM28HCGHwA0MRT1sok2%2F2UgvpQi0%2BnNfx8WJ%2Fm4suo7xngQIKjkjn5ZbHqIlk%2FxHNp3vttVwc3g42%2BAaOWf2cFxtJTHWXQPP0xbOj3voEPm3cOnmADTULu71bxy2aVQWqK5%2FaI91R%2B7Ct4PLmz2R1wngTjIRKMyo4TEjnopNaaaz4A0X8hzQOq6PrSQP5nvKP5dxX%2B%2FO4pozTKJlxPymfA7seG9ONXUJTFyiMtRYHsLDcarcVgy%2BTQxQD5KAlnMzg112DYcik%2FRAAQINM3DRpI1L77P4rVEJjhYdTWAlk2O3YSHjE4kvOET%2BzCaVxreDG1jOPEDNFeWr1D2AB4TxEBwkXrxHaw4Dl4u8on44AlPhGhRovQs4YAjHTsHxtI%2BZjJglOroLVuvg6%2FMgQEXMHhB55T0FTLn7C7QfV%2B5nLN9Cpuob8GVRSi2gs3TXJsKvxze8mJELHFHcCzo8cLduIVFHhNwsjnaZfx6Akm50sqHMoMjc%2F5dFpMdUEJsayYt5TaFjrHw4Gh8mRS7rNH0bWqQCMOO5rsYGOqUB2CsKjyn9E9orfBbseViAcyxbzkJqgs7qglw1eDpRu1Tv06nc2dtWvYcBkF4tDuv71yoYBdFbgpkscgZwHZ%2FpOk3DPZzPNfKq%2FUDWWQrfszuv3U48yQIUpMrtW%2BzoxkfCWOiRWHFPRq9m5fWo%2BVftNeIvQa0NUv3h2cvpFRbcCavx9MsApvvwBsjf6OF8mY5GRYehEYgk1o1p8B2Ec85u6HS9jyXS&X-Amz-Signature=ab97d1322003e4835a75b8f9d41f9f721fe0c3cbda49051f52ba9c1cb37bec22&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SX6ANYEL%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD7qGZUhRsuqPNsxr7%2BdhmaoqR5E8vSqq4rig67r1dHlAIgN5fPwCNn4HwAZGRKPt3mKqAPJjGD6AQZAbeo0V0Ccokq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDIP5Du7YfFO7BFGU0SrcA98Dx32FIvX1%2F7FScEjvhZipfY2fiINs35jJ%2F6YEf9YGVkK2KsTq2lFqAPbdbQw1W7kU0YopOeL6kfUbgCTceA1K%2F952AtNoPWnq3DqWv5Udm0nMGS0Xpg2EIGn%2FR2qIFZgrj9xLtdUoM9NQx%2FToLAVXOVhPPYtoDItyLvEZMdPMEynrlASWZkcCvZlZ%2Ff9HpxdQIGe5ZkuvR2WCfl9q%2FO4iwtqg2s4WEvM0axI9GqRNFc3fme6PW5JSyRARE3q9unW7ytbr4KNiFZJj%2BjmuitemDu7JOLGnrgJ3NcGjx2K36TXlP8%2BIXYx4Z4DnZEwKrz2MF2d6NxtkTIIjUl8c6S6JgSm%2FUV4k4mfA4DH1YKfeEUZyDtWvowOois%2BdXz6TE2Dvg0N95cNlmp5hsLQLKb%2BD2lYDwqdcsugZlCNmWQCiw4PYLMdXqHWVZna9kVfSGHhI%2FxDbOhkCHJFIAp6d%2FzTVcUPtSp6cDdINupGf5ifvNaNFgYvPUIApUXfK0zTWwA15hM0NlcMwhphg17sPibVEBshN0rEQ03MRSHjSBMZy0B4k15aVrPGAaMaEq8GjBHkGnsdroj3tmYZYNBp0pAHj6BbJXRgy%2FN888dXq%2B1KKaV1ugTCcAtBqUNynMMrztccGOqUB1Kb6M4Atzcxw9d2mCtdDUrT6msPsJBO6QEHpn0mXoOQI9EpbxQ5rgPWuiAZ4w83UCmuOAeJoJQW9Dgqxukh5vWxO8ViihbRIaVmloNicOR82kbdMsIE0K0iJGxF3%2BMV7JNJTt1SN%2BQlfEVwVf%2F%2BnsRCUSkxPmZ6icxVec5gKYGeQ%2Bk6f%2ByYr4uwbleXcd3cfC%2BD3dfWIe9D%2FdIbNRBdmOGS%2Bvojt&X-Amz-Signature=93fa177ea585f1b48ff8198c79fead112be0798cb42afebef0d31b76ee582b07&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

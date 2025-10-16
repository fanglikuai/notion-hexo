---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QAE4FH2A%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFOe4RIEDA1DAOI5RdFiHjoOTdOY2iyZEqYhGMg4jZghAiBQrCgfd%2BOGLT5PZCCl0ZhOfG67vCJOuMQE2epnHT8R5SqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJ1H7n%2F65QDNeAA06KtwDf5f96yCu6LnRAqjCq2SfORvD2huqSRxX4OQkzzv0iYs28%2BPxP5UhLIFyfA%2FOW5Y9NQXfEa%2B%2B00I2xu4fBVHz4AHT%2BWSwH9Lu3mUTuMAyic4DiGVEyEDC59Bc9p6g2wFSi7Xmz1SsRqrT8jqJ95wCVF7KH%2BBivSm8PuIwb%2BbdzpoZrD4TK%2F7InGA3WsX8ZimEaJP24hIGVjTgofaHVa89ltYwVepi52kDahX5Iz%2FawXshovXWvaai8yCWNbeXwFGuYXZOvB%2FaQefY6akH14eTGElc%2F3kS4Ci9dTdUdrLh1A89WPhxECR6iqNYm%2FaNYbubLSI9IR05vCfqSkSYvK37OsL1%2BCypYIT%2Bc%2Bqece1ns3Md9P4D066ggeevu8pEEbaFXxfn%2BsWd7aG5xsD6FMZZ4G9MzK4lcCrcPdGlrAtYCpPL1pRbXc3GOC%2FB86ogvUay%2BYP6fnrDBubRnNCwgv1HdEuksBQGyrgxyXNKFTawlmDImTm250WwtRuS3gnGIuynvREt%2BKgypStNu%2FhHOM4JLAN08sL4IgXBkDwWpVf8%2BbKVkGbtyrXkMeYn9d5eklhBTUeBE4ZJz1agUsORNX%2BHKGPnq57ipoyLiPsbp1anyRj28OHoTi4jhCyUdE0wnZ7BxwY6pgFvlya5N3xoKjiKRBMignRze8NX1RJZwbSUH%2BpZA0Cm4VWP2DXgvZbLgewkRn2Y78SKUAZEsrmuiWqmNnztpZQlt%2BVi2BErPefLwhz9%2Fve69E1XBNIwaWAhR9MKkwkSO3sS45O4Tty8g8BTrVbqNjxaXuI2mGK25pGe1n6qFO%2ByJYUuFXsJHsn0aHraGCHpgTZUlMEqq4QkhS96%2FvsNXH1PSd%2FOhge9&X-Amz-Signature=a5d958d4001c090d3565dd7aaa39453a16fb648b0916531d32ece273ccd1ec7d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

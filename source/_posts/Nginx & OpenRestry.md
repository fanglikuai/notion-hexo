---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VVG7IZ4%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIHrV6zfO1fruR8Dc0yskLzEOCARPDbDeoIZ83GFK2DcMAiEAq88S4gsZdY3Bxn1uM5rpXedXSWNXlO27fT6B%2FWKrPmoqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHibS7pCAv%2F3pHjJYCrcA0fceBNHDhu6hNO4tDIBykgzRONExPcxlJ38fHI2TLSRvKmUiId8VyyciNVPPI9GDm%2Fw4B6F7vl%2F0Enh2P2qlJnKURzCyAjF51ptD0Dt%2F4NO%2BT4ymQC2PUyD6ii0ZjQeD4RVn3trtZuyPs143r5jYpnmIZSFBUZIH94YS9ZJDCjvGLpwTEzGbCl5OMnwsebDeu7OheDD9l%2FILajZPlfDORSNPSDRaxkSpxEabOz8lnRRT0o3QegeEnI3z2gmY9ccJ6Fm6gyJ9Xc%2F4neIEt5aWh2VSVIK1XBKZ46I27lKjkay7X6fuiml5H5r%2B5R6fZ0ACBrFdn%2FhrRo5YoKEuubC%2Bbjqrzgk4Xam5jB38A0%2FqWZxX%2Ff6nmjq1mbqPW1XQQ38SaaqapOL9nEKhTGV6kQrHHwRzORaRe6QtONfPW7LjUSsXWE5mTbKHcdeMaCTfEYbNf8%2F4AwAAO1UusvYw0zaVNl17B6FI%2B4xDsIfQno9R2S2xSGzCFPTwY9GrnveBWegCaiOxb56V1q80wa8U%2BL86nLKeX5j751UZAuJ6cR%2B883ofPu8LoCec5Qj5qTT4HJNZGIgqrKbNlnKuZ%2BDWJiASCuP%2FiB45qOXIbWQrkt23x3y7VdpGVL1i8pemzISMNzetMYGOqUB%2FrAi%2B043It%2Fp14e2CEj2YjU%2BfaTeaXEl%2BTid6RKJsVIktYhIEHulT%2F3bht76VSiEmWRGlZHw4ePOsElDG6Ezj4KbqtnCRMtaC0E09%2Bc47MoGUbBsHXbiDckdn3Mh86Gwg4wgOvrysxD7dD4WvCyBk5C6dyZserurzxFJxZpHPRCZqybU4cq6hicTjwzn9ygwYuF2oGjp9pLK3Z2Jh7CTL2qGZyEh&X-Amz-Signature=9f1864dc99cffa4d9d7475a5db1e12aa82e6cdc73b111829736d30ff4fde3fa7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

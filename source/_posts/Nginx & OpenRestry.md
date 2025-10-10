---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSSYRMB3%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCKoBQ1KPSsj%2BXQEb8VWOGRnnubsfi3Mw7YG7efj7I%2BigIhAKj2vy6nvfM0rminywuXsVEKkn4z26jddMt3ERj9XX95KogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwfcmV3PNOOKTqoUp0q3ANRD3wm0bsfnf8yenmNVIQbBM7RlS09kxyLIUvnvYmbhXhAD9NyE46dbn5FlY%2BvlT8Tgs%2Fgcg57Z7FxIBQhdWqB28qQOEZ9nY78DB%2BBgl7x61BidKJx3c6%2Bv5gvD0K9ImxQuh2%2BLJI6qWe1d1QMu2UJXXlswK7ilT%2FGda3w7Uj%2FvA4V5HZZYc94spLS87VZHqvdFNBZxOCRLd0nl07P4sjFMBSltrSPKLiSCYTPA0%2F48%2FuZSdA%2BoxRBCURbb5a7TfC%2FcSlLNMsnC0n0ov%2F3UWvTBYP1zOpualuay8dsQAdOd%2Fv2kTUTVADF%2B4VBFffa59ymRrglwNp9xlwQXW0LhSmr7BYncq%2FdbXsm%2B930U7aKzO2hDBaILN3dRQwaoo0hwf0N9rALCn2mm5EpY2lILE5%2FplTqbsES%2FlTfHegJPa6sARTI25%2F%2BzZQax4TJecc4btqL2MsbG%2BQu5Z9OUQMNN%2Fl0O6HUgxWMEPoRPi5UeQ6GtktZs0VGPTT3Wbavny0A2cRXqaS%2FUFIWobFDDGwmRiz72ZYwkwNre%2B0g2fsVA08n47MvHz7tj4enmyLWGS1Ra5FkHMI3J4BO2NLB5QUjpEm2hG2BGJYcIQgEPLaG0GsZCADbs0I3lBskoUv4tTC%2BkqHHBjqkAQfPIl0vb%2FDpm8CEDJ5%2BVrZv2Ykr0cNjU%2Fy98y8c4Dk%2BvIvZTwknqb6oMKP8KOWwIypYr2xaRBc%2BA2FRtIMMGvh5hwG1JLrOIZHRuCtQzPx%2FByhvTP6ZgNDwxSVtuB2n9yz63xUPz43xzmasZYCrf8PT82ZbWJbhdW89H%2BHH9psgB0uYGJ6crkGWEL6EEeBq9U9%2BWHNdgGK%2Fsqk%2BxRqP3tuNfWgr&X-Amz-Signature=c545f36e43c6de44246bd548c6b326b47339e6c05f814165e0d7fa1b13b02bd2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

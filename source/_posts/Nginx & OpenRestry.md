---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V7XDDIMG%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCua3DCzi1i3glBXs8jcJD4PnhZfRC8VYQw2%2BrF2oR7bgIhAKJYA7v1u37MRDdcF046n%2F1GqPc1ZWbGgS6QTmiF8iEmKogECJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx9nF45o9mKIChmhegq3APLD6qCE%2FGJab6Z4lc1MQ3Ge0COiSR%2Fdz%2Bywl5DF8hMiN4T5Y1ks%2FEJpACgTT7o3Ty0LB56RPiOA9lDfNSHj2TYU1HbEEl9WvgaBZBplzUtvCELyBVmZ47CNszLDevcUX71TNa05M%2Fu7FHyqo01MPnTG3rOdrIfg%2BdDiyK8OOgk4vcOjNmkeTSdu1jC3sEpT14J2ILmQ53aHJo%2B0cV2WZUpY997vQ9y51wqo1z6qpD%2FKpiUljnDKgwhTYNNsRxlBTNHcrWrAsUL7f7w0tt4jxIkCl28GisSzLbdvFyxgWOnL%2BOU5%2F7N1%2FzZvhO5I6baS5i1TqIWKDptBu2G5%2BUuuMeVulM1fpUrUO8Lpx5hB4dRzXq0xNimMUPuufs9c1Tgf1PWrLARcnoqEMExnz27imPFfmuvJ%2Bi%2Bai4wzzRUXCSmDPJLfr5GIPToJ%2FM9ojLABpjth2CJjbt%2F3DCQxAjNpgWVdDFcBdxQ4hHCOw1NKXaa4HtsLx5k6fRNzgQ94Q0XqpaHcoVySKFxsKpOnay4sVHL1fGtYwqMA%2BMYXlensb%2Bz7SE%2FhGj2uuVP%2BF85TdM4BrPeCcxYZ9bOAK5AuiY%2BIUT6PX2UkkmRK4SKcZ9thd0sUuSIsao0mIeNkUGVozC%2B3ufIBjqkAaFtwc8bGATjD1Sb47aIvTjv%2BTH%2Bg%2FphAOUJkpiL9UZ%2B075wGCXMteEsoe3PwcY9ZjQLxGb0hE0yCklIc0AXyVszhlZs8ofsWGjEGSwytsMhCsGV941PjVXeVRp%2BpKJyQ%2Fm%2FT28e%2BbH9hQ%2BJ2ZfquHaKfDDstwGPGgjkjebsIv2WnzV%2FRQADyVN4BIBHD%2B0rmz1ucgODjLH%2BRN8MREtvKGv2PdM2&X-Amz-Signature=8818042063fb44525b27d2fe2e0d7b3484172816a68bbe07767f1c2c17752bef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

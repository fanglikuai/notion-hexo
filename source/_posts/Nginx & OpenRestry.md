---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QMU5JOSG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGUIVHpbC0KEbavg3wUgR5edQ5jDnMC%2FS%2F22jULbu4OkAiBT8K923XvmV%2FCVahd5Xk5ZWRqHCQLGcZOUKBVMgXRH3yr%2FAwhlEAAaDDYzNzQyMzE4MzgwNSIME9aBiHrsW5VnTzC7KtwDba28hA4LjD1JLFrGswDeMZ9E5%2BrPqT0b4mko6SgFtTQVbhGoYDgRCfMtGUfA93Pk1CWE2nolHJou1a4LV5vFQRn%2B38slpUDLxY8TP2vPDV76gOWCTFuAqIsC6WaStEW9mdgrbrFlqJJcdorFZC%2B5qNHWct3YQYhBSiIXs33AbaGmmBpA1tO9JISpyIpPb0MUPAr7n21SHn9R%2BGk7f2DNs%2Bu816%2B5iqAqQAgkmF0rsBLKarmEk9UR%2FcU3wT1i4pk69LsIMlqzxEv1rcjqi3Pt7Rw0EgLMfD%2FM%2FSHwmmljakRcMTvfatuKsdZArV7TScdcrmN9FFhfUBg54EUQ98HL1pgoibDC6Jo7PVB59YnzBzOpCEKfQOdnlZqnwMzqj5XE5NXxZrMyxjJpuHGA2BP8RTqHZIUz6O9OkGbu5ECv%2Bujjt48ET8OCUjM8fusp9e0hl0DE2v8v5M0Cdy6JjwlkELNOxXVjnkA1l6JXfs7UHjmc0Cfv7MSoqQ739o9eXowkjTmxuGcdyyL8smpvs8oKAb0dH2Ekih8c%2FcYtwlBJozM%2BGYrcTERs6wR6%2FUNNzgCPylij3o%2FljpwR4N5TMlkJTJ1Ji7hvQqjPJP27QLNL0ASzGpAG5KpiULBpOfowqLjvxwY6pgE7fRAsqRWEo2iSA6CaJ8NDAXsoN%2BKYsuhhxLUhdUPDZelVbh6%2FZ5P2jLMojh4ZHrbszseZ1bF79bPU5%2BgMySDo0dlSf7E9qgQLnLHWwWnv7n7otqEloW8NeJvVJEsQBAEp96yc8bbVG%2FyUmNehcrLMEXQEPqc3pa2MPDPEnWTV9pJ2bneUFPvwJVNvnnw%2Bs3%2FFeLk7Z4keEfQg80AN8J678E5ewvuy&X-Amz-Signature=45d63e9136f8c33d990f323a1cab24ea3de29c7c86ad04f0f75380e8d3a850f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

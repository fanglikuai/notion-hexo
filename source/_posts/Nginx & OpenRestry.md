---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667D7TVIRN%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T000052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDzohNPtDtlEfEdPdDl6E%2BVFwJzj9RLSzn0XlYymvRDggIhAMYtBT8TuIpNw2lW%2BJBcuAQXQyGnnlljVy5PbZclw4NKKv8DCFAQABoMNjM3NDIzMTgzODA1IgzYgxBtpHPOLltm8d0q3AN5OAc%2FhmUSjwxJypT7mWyIl%2FxAZcr%2FmGiadVPEq0TfzZwm%2B%2Fb1Dmx5KkaLnOIWs5CbywsdKr5R1pQpQN1L9dvIZLeeW5FMYmxcqsiIEMKyeZ49Z3Q%2Fgk%2Fv3e4e934tRVjBx%2FcSCt%2FomKpO4351n8WrvRc%2BPo28SK3ulHao4Ke4Cvo4KO7WDn41jfqtNGcVLKk9crk0DSnlhEp5Cyd29yRIpUzTG25%2BxtOJrU7hV2%2FHluqEHt9MBzimiZ6f%2FKwdER%2FSGO1QDqIp6iCZ%2FymMeBgyHeuY%2FIfbAHHVkrgZCzYG3CZd97dzVve7103e4SE%2FcS5cJY7EB693FdiB8mImicOuBtkZMXhoYpdauwmrOPe9V%2FR8dZ%2Fx0%2FruZSRB3YKqWzhmemFNUJk38LNCnT5uRjKzpRp6TSzS1f7%2BNr3Ts0Hs4Bb0rOP3lnhs21TE3qeQhNArgNX2h4FUvvfWdY2PlVdvwsuoWoDJ1E4lMp6ZB5qVYv3TPz03VXb9588%2Bkn4Th4eW2%2F9GDgiXLbosU7vceKOPwnDbRy%2FtIBCQTtdJUO3v7Y6ikcOU43eOk8dnfifdoK0NnktzckDpt0GQ6Ztqz3nZoZh8v85yPxy1ZZ%2BDd3tKmVwrf57I0UUI07zZxTCN6erHBjqkAW8cOuyoSk8I8k0BqSN4GmBYXxGiP7KmSZZmNVbktNwWL4WpFm%2FTOHmnzHAVO4A4AcehAc8tz29oVmWj0QHbobRnk%2BDCfYYm%2BKpXick1EfSWkTXGOGO8%2By1nOT5%2BtU3ttwp1NYdJ%2BWDzzaLcSRugR8gK0rg3K0Su%2Fu7dPxY9x8OzoegtuYY4YVeQh%2BL%2F07T4KI1TGbFpjXOBdYA9%2FYSIcptSdNL0&X-Amz-Signature=121fb2145827f1a857c3fc0e52324b1c3d8323296c9efb2547c6547982357122&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

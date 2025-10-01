---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V3R6K5FT%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCICK%2FeuFlPXS5r2wAaUb1Vq%2BDTGM0Jt%2B1%2FMKHlA2xe2h%2FAiEAmE%2F666PTEA7JFyrq8W94b2MNMtdGkxbt5mfw4227Io0qiAQI%2Bv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDImjtVKN0qgI%2FSuDkyrcA4XvqSumICawkTXwf%2F%2BcWCVGdYwdhbGo2DJTb%2F%2BJGjH0V%2BoXe8r4G7IccnQWOCeiNRsLL7wksW8wxXMA1LY9q6kcqyqvyxndF14Frd4iHneUk9o6n1lF9nNMV29s%2BhOHzjYS9b1ZXvl6SNuKZ73nS1bV%2FbpRM%2BVkHlOsznngHwm8KTezw5n5LgLMIimKuA8nHifDxTh1QSQc3QPfzpTEHilljuz5ac947H%2F22zwnImID%2BYnOeyj4TQI8uHQUgBOYQwlRcsQizPwkf%2Fm78aTJzTki2c0QYkOBc7ucY42JVMI%2BxIy0yQRAVaHU6ez8sld00gFXwSFKnc07Ew5Ms4dLQIFqA1ppCRfTxw4XuCa8FgEWnZHSm%2F8AtDNki3a%2F0fQ4o%2B9Bc4trzzocglstIgj6Kr555dZuToTnJMzCKf3GBvnRxDR%2F0qiK6khFtZFn7RyK85d79UydOe4U5UwB1LU9GImvCLjyqNMgsm5mtUu20%2FpzW2eRY9j9%2F93yYax5F0kvjkmp0D8lKAlC9ySWS8bEqRD8YSAGAqRM9Co0ZR4ehgywbOUX%2FUmse%2F0n%2BIoUJRJUSbg7e9sszbq7KpNsstjYnkLZRlC%2BHzFUWr7ayNgXCH%2BummAfzx2sMKP%2F5UbDMMHq8cYGOqUBrVvxRCz9DVxAa0T7f4eO5J7nYe%2BEHt1icM9snI0Sa4xsQoz9Ichx%2FkhQ55wdlAkKegkNnKuMefkU1uwZj89ECNUB%2F7dc%2BygP%2BWbNqm%2FE%2B3rlUJaTB4MMZ3lSz2IUhwR2YNoKebFkZpoGeMRVFiPJrCxBNBVSB7AR%2FT0U2iM08b%2BjC8Ng71f2psUbRTnDFeRiyBkzdKinNatCQoOm0B8K3h8%2FTxxK&X-Amz-Signature=06f651f69f006d2d90d4c381456a0d1889db619f0e181113725d0978d457827c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

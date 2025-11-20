---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2GJBGM6%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJHMEUCIQCwdZSUq5QsHSya9TPfCaOFofaL7fHr5bmPoE3D1qkaVwIgMLLVrJRyBUV1GPRcl6631NGiaTVITNRJp8kEw4xSa4EqiAQI%2B%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLf6A0STaMO8DCp7qircA2l%2FRTFnev5OrpoYuOfqzILwNS3j5pPPZhIhHgZKnH0yuPC9ue8kz%2FbjvpJf8PAmk%2FXfTOi08Vg2cWllvbRfHLi0rL4YQmGhXYQgJIcdewnvO%2F4R1kKfckF2%2BXvY90fXXLiHdBLfh24L0XPzSzvIWZEj7c8DaEB%2FKQX%2BfsCAV6rfAJjztDuxdVFPOS2fUE7u3nxlqPk1I4cX0xxr2zBPThbynXfsCZ2WW%2Fsy9eEfHWpSZTGtLJORAgYflNzYDcPCegNom7tX2xXbsRsCpJBCnJAkswkvUmO%2BGS48IAfy7n7HTw0EQxhS1NwasXvx%2Bo9tyC9KzU5k4JuwgnjowNEmQ0S%2F3khzRbybUl8PJbgZiNqvzM2NSm%2FGJLtASuhSyNrIehrwAraGwv16TVcvzg6Tc39DTl74VbQy1QmpBwKkI9O9itQwap2hWDxdbwmMIU%2FL5wvxhr9Zt0VvQ9EHqgHdOkwaH%2BZBuW1c6LZ9sJHJCS435SOeH1aHj7uoXP7Li9%2BCgmKhCNOyqEaMZm5fwXPVlR2P8NgC7UkauzWWsTtHKYbzhmjUwMN0qsX80Eg0fGMSgY%2BnBwGazQUAqlq2BHKjwCSFzH5WFVQlay9YIDU7dNAmNflxGRY7UAQ9ybmhMLSm%2FcgGOqUBuFRneosnDYfnusw9SCFchhXqHCLHiFOzsN7tbg0cFV%2FYd2dQAvH9o5r%2FK5x2dfTmFCoHRorXs6SLpvBHQi7hpSXAZsoZu45L9LpFyoC05r3My7htR1OQgkdIKIMaNFbeDw5cnmST%2F7YB9%2BC7fLleyHZUm0JTO84Q4BFnsibK%2FL5GUd6xNPRrJJOc9fleEoG67ZMdI4GeGXrkVuza2YxgGvx95t83&X-Amz-Signature=8f2d7234b59ea63eb7bc2932633f29926e410602c93cb2952e7fce48fb4c201b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

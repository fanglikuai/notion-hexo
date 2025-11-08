---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666VMWVIAT%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T000037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJIMEYCIQDMmSTrLTPg7XR6NG3NEIPWTbX5YM7RXS7zMMp9%2BVy1XgIhANw3eUTV9%2BGHVt%2FzqiC0fjCvErXYlWJ4h7c1oC0fhEicKogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxkFcudclag1FBfSw4q3AMOT6LUrEl%2BbuMOrPhQ0x0jNmAher0cgdwedY9v78Ly3a3wseSeQFHt1Y%2F20yGNPG19jC32xe2143%2B%2F7UptqyO0sXqBkcSRkKy6Wh%2FHLfedEvByptt5HJeooxpMcydvpUQB8C%2BVfLkRh%2F1DGqhvgP4mV%2FJc9yuB20dngm5iF7FwK7jb%2B9%2B30OIitBm4L0XJJEfTtl2Yrd2smfTwPiziM7t2qT5cWruwzT8Pfe0uoC3z1CEwhkZyFcih8JPwdSejDPQc%2B5aWAjKRXSvD2rk8Z8qeczZCOAxfkFOheUw5eXonp%2FCrY1O1VqaQo5L%2BnNC7wHS29KRBx6tK3j3Z02RKi%2FFK3LvYsNF5hypRG1QhMZE4KKj3iVlV8GT%2Fyw3j0es67DeB1Tknafr9A1lcsUOfQEMIpbdc4%2BHlNGQoaCU%2FsFO94JMdESn98xAvvqk2pGStCp0BI568T4vuvHi%2FnSs9rkfQbxEPXDIDDlI8B8Z0Csg76RRWNq55qhv3HMMW2qfyzh1o2QB0rCCuPwuf7%2BGvBpOBfuuIY1PMCPe%2BVHwikzmP3mdavYgVJTruZr8zDg5Ye5cDds5DkGI9ZT5UA0F8y6t1AUzj%2F7AfSPSVmf5FVOP56BCnFjXYxCMibiO0CzDp%2FbnIBjqkAa5g4P654ratxWS5cgWy75pfNxhtnDW8ZUc0IFz8GyxW6k1O8FxLQggig9TxZ%2F9ljpt7GcnWTnmtoQbz4WcUshXfkLXieH1rtfqk42J0piTMZZwZteHJH7YGhoB%2Fbz6hrnq0l6sMuoZYnFAq34RZT%2Fgg3QEW2mzxQGfQ63ink72EdICx%2B8qVyT1YdXFJUNNG5Tsta4YTjIt%2FgAQh3BXhwdVje%2BxX&X-Amz-Signature=4cef1b43bda80e6c3a77220e07e4d8167a2081e530dbdcbe8321212b6e6d2f2b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

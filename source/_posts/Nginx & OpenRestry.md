---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663OP7IUXR%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJIMEYCIQDJq1vHvIduuIYhvZrkEeUm3FKYyueM4ZVFVqd8mmnlNQIhANFXjgytbinGKHM%2F1nRmbZTM9KWlS8dH0FKkuEJnA4HhKv8DCCQQABoMNjM3NDIzMTgzODA1Igytdhag5JXXWzgJsoYq3AOiRyDK6qJooE8kdqEBSfnC0JtPA0MoT%2FsLBoERjaXrldaH70oU61Ckq2hnSKINGORvaIiNt6kV5gh1LAlN2Q7%2FFa4WSR1ddnD0MtoChPAe9zljPkFORi9kvxPd7LpgZsrW4QvhrLPc6czGaBbPl8gIthBgnUNItr%2BQUN2mK22yYrLDUPjQbv1X3GVO37inxZ8OJli%2BKmyAs4gEvwAWsqo0OcrYdslIC1S3573UZkepjzA8F9EcuJVMII%2FsxX%2FA%2BfyhRHUVIIrlxNhhSI%2FJ%2Fx8F%2BU%2BMMaUWiUNhk6k4AtVbnBzYwyxo9vCaJkJcZ6o3bZVRxVo5prPGdQtTZPMvJKIe7u9P53K2uFWgcFZ8yn4KnH1UpKMnmmpYD1GrR9cKRkXkkChq9tVRtu8O0wa9zvECHQbnXSaO%2Fe8nusQ%2BfFxnEMWrEnoZEknwcInjL%2BTwb5rByt9NtNwD8a1dc0BxVFntf2ghq7jDKDazU3JS%2BFm%2B2bxR4dYHRkBQSozvHLl1GWvCM4zt9w5W5Cu1H0wQKlBIFdyRutwac%2BcAdRAvb8S3dQe1ysQLGmsPkkg3S%2B1OfVkqyfPKwvHX5Me6to1aflB%2BOvrcZh7ORJ09eUgdiPdxSFty%2B0J8q7w4CYWMSjD6%2F83IBjqkActI1%2BI0lNzfrA8hv6Kbb86UCwX%2FQngL34uMqr6rNmQzE8wYLTwWNtbenjWYWnPGNd8DxeuDYew3oZBFcebNYTGg8ozgUoB3adR794G7KtIQub5ZPDGRTbj5k5FWJRsHxTZ9OyVsVmoxg8ze%2FvIsKTva69wa9soid3jii0IYEKazVV0eY5BtplvLEqs4xCMI5B2YqKMGBXltsAOrlW9RHE5lRzkW&X-Amz-Signature=cdfd63777e856ee32aaf4618e21eabac2c6c0063ca5db1e37e47748dc7a78849&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

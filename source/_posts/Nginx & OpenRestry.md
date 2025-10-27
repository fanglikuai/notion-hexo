---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667QJ43LJH%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEsaw9FOXd7mvI%2B2H0wL7bW0HIeydzzR6ztSlNxmUzDxAiB7E5y%2Bnbr7f733Dzw4Mkd7kW9Ser6RJZbhtJSWfHCjoyqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIML3ZQL2AyMnW2NJ4ZKtwDkjLf69FxZ2yFsKqCbBll7Pa8AoF6AQUJhqL9RBjbAV621AwcbZAAZa7Hby%2Fk%2BrL%2FwRtX0pde2gZWq6GtpIFqQ%2FIo8Edm8mgef3P4vibHDZqNXa6FaPunNAfANgTgZzY2UrjuMq3jrMMCrc3dUjvVDenvenc8fTHmk1bg8G0FQl8yY5MSMcn2t382EjTGTIKTR5ObD8bHoKPsvmLx8%2B52gXIq2%2B1JeApoQLXXRZFtWKtHF3RYj6x1zqZJNpwQlOfMknloafp6UitfqnHcdsigvLTesRVp48IkPyFhetmWR6dOsGpRMXfm16mlDF%2FE0DlBb8EU3D%2FjDayBQqHLicy3%2F2h0KaZWkvuCjOitxaq40fgQBXO92IR1WLoS9aGDLIDttFc%2FLnZ5rrlS5TWStnCJB2LDgg5Y%2F%2FX7LfcFLQr8GdvF1Xp82DtRLj5S4BF9oCp1C423VD%2B6NVbFS6Ba7OJal5xq2fxtFLR29NpEa%2B643w2aB1IdzC7VPdig%2BVagqLi28%2BnTuq9RK8ix%2BdBeHfIDF8%2FAHvAxtdagrGTUvSSVFWJknaTpnSSLwjnK9PY4ACXfhpR2U8BeHb2iKZcKuWsyDRg%2BIWCY2hRwA5hUqK17J%2BOVQHLYyD2asS95Hhswkb%2F9xwY6pgFCY%2BjbOQfHg4SC95AvgIgyvq1Jx1UtpixQH%2Bccya6OtGHkf9C2FJbefKJ6eGJjKRje24FTXhzYk%2FxuoZjOcoqIM%2B9I3PShULOaKc%2FNH%2FFJDHf8B9e2yo0GPu4zinEYH5MdP79d9Aj7f0%2BgkaKnAPDt%2Fr4bqF4Jj0CcQ0Vk%2BTfraIi5bI78ZFPhH8%2BtZDKq%2FeCwnqbNoXhfFSEaUwAjii8nsMAwdAXK&X-Amz-Signature=4bcd9e90a2d014cbdb4c455d5544528d700b231e54dd2473c56014a24b9d52de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

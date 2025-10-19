---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VB5U35AV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIBH1gmHqrmizge6SoPjvoGPUyIhOVA3Y6AVMUbNf6IEFAiAhgpeS2rNPY4CGs68gT37YcTVo00w66Ul4JpxKGSqqcCqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMLkrIwIR5OaICxu%2B%2BKtwDLP9VbSI8dZJLWqvF3iK4U8D49fS5kpbwU8s3pt8XNi2IiM3s7m6K1xsxmJ8y84YR6opES9eUoM1%2B%2F1hgz3nF1VXcQk5WC50DhN8Xg2LY2J7TT5h%2F6iOiQ8EvVMK314IGMx%2BIEfrBYqIPA2fGQWZm9zAcYv5XQrXY0hg0Wi%2FVw7gWY%2F3t3p7znTbLVqWuanCPGgngrZRCUUGs3kAILzRmyJCdyaoinhTl0II9JtHz5VcWC%2FG4XRk71xj2TFiJZISryAZVo2%2FayWAprhlFwJ%2B8OBaVOqUhzFv%2FL3OCKVk%2FWJpW9QPRnYRWwWEON27aKeJVJC8RskwomOb8PlOndz%2BX29r%2F5IHpqmeuHmGUKHK3bpjQlGsjGSyTh2uAY1Wonsb082u0DbkZB4ksw3H%2FjAkFe7XIjtpJI2SaHBAj%2FAft8sGo1oB3P6qKEVnZmo7MKpDuH%2BYj3AKe0JUNJxhjM6lHPszthbBfJKJ2OnV3eq7CxuxrA3OvwKrRzzytRa%2F1oGFwyYzT8thY7%2B7jG3eoobNqw6F1KXJp%2BjEl7xHioUFKKIK2dr7cKtLkjeDun71qoMSInOwIE9jIdvQDD5ofxWVdF8svdSfFWdZo6H%2B69QupR8oyHDlbLfSbuXlAt7wwu47SxwY6pgFfC8MwxKIwgGYCP%2BmUUqjWt1P%2FtYZR%2F3pingzSUBJqPo5H5WOzWrQ1th1ObYlWXuENdJ%2BSIA1c2Tu%2BpzQQrVVCRT878NoSiP1t%2Fhjo8gMWJHeSxaTtVmEyPBUo5CDrPqpMC48xhBwNIA7q%2BK5V18TKdwzM7eX1B1XoBBsxTQarBLUE8xmQEcROi%2B91rEgqbToeDT98UWXTnMc8s0z3fsMgBB0slXyO&X-Amz-Signature=e2d8cfeb61e61e962788ef20c69d06bf15802d7faf2345a7d34077222aa64ec2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VHMN7VZ%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T230037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJIMEYCIQDCggtv5zDkDV0bA8Z5RkLMEm%2FQKexncE6swtvq8bmaJwIhAJXprM9MtF0BTUKTIadyolOYS4F4i%2Fwq8zHnlI7NTLM1Kv8DCDUQABoMNjM3NDIzMTgzODA1Igxj3w3aM5IJhzJmGg4q3AOP3L%2FBAo5zeGUMwTQIxowtoi8oj%2B98zDS3dtc5h2StVLwLgHpgXizmAkHpvZYyfGICSmEoh%2BAOBP2dhmfCYsNfQ2PKq4JAvIGtFppGydJBdafcVOBKu%2F2xKgFFGi22asnbngA%2B6Q4Svf3nK6CyMzt3JAEnVpxlwVGO9LREDIdvj5g56P7N8dTDJZIFZIT9DvIgtlhf9%2Fr10f57VTWktCCu65Ig9TXbsxwMVbuNR77nCbwI3aD%2FbEsQCEQtGZiSjznKnUINC65UEU8sKz%2FHvMBWi8xUlnItZGbEKpmw3xotdEktOe2GExnPeWLTNyNuQQ%2FewBH68cvsbwV0Om8S3YEhsgvebPfVop8G7oNkjtJAiLp1%2BygiDqrSDkWvRkRtColuWpbWtl8aaiolu1lWYrpoatB8Qsj2On%2Bben8Scxe7imTEJB%2BDykAoRn%2BKjszUv54TS8Bba07oCzjRfikObhzLngQMIuf8Dgo9qkwt9UvpQy%2FZTehfxRhSgoQCo95qACktnq4U3DTS3N2eOjYtKEqf0PrLZRExbeAljT%2B%2BTUYCqHxM6W8ADl1rRgJPuTF9SmSMmKw475UwR8Coos%2F6cxe3YuLkY0cHibSpUnAQ1%2BbRT1bSZreZMcmn8j%2F2RDDJw5nIBjqkAaqMRNWSB8JSpmQ79nT5IgFIz0Cw1ZtVMwJxGQzc34pEPvc7%2BAFM35gpWyEU1OCprcMHsQUmjlZy9Il71wmrKMy382vFfDuRhgmqufeLNvLLojScCKANSA%2FpOrrXGShGBE8qHjvWz1MAwtGJfa%2BJ9tFNg1RLJwbDACVTnjqIAyz%2FfjFDC3vEx3lnE7%2FAhZJu%2FRHmRd1YCvbn%2BSXWSg1Z4A3tarua&X-Amz-Signature=2b5b2032347f7dba0e34ba89a40580e500fbc75541b9521b8917a968730c7ecb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

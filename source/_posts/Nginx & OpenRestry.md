---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IW4DX2U%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRHBoPktg04TKMOXxhmoxpE3vJhZAyv7BQcCb1%2FTnKswIgBxXBgxWio%2FEUbGgtcVC8JAEE3UxfL8CMFNM%2B0dytbZcq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDPnVOl09xV629S7OvCrcA%2F7Zbjj7T29nrIoMbrrxPR7LlH%2B6RrHwJ9fj5O08MT04HLfbbx%2BNokrwZYBD1iCWQpSFeeHHS7jb3sd7ijILPv3u9SJ3gLrg45Hk%2BN8xF7vIgp0kqWcH%2B39L1li6amOkR3azW3Dg2oWlL%2FRQ5koXamy0%2FKBYyIBdq%2FDy3ZXCcKFvpanBEuU6sfHaWrp1Nq6H0uqDDd%2BIHa24qiOIvvwrNzRbh%2Fiq4O%2Bb8Hh7WCoJSXzqNDHa4T9l2%2B8BC7BQEeIweWzG5yphywzbPZquYFg1shQ1l%2FJrHC3z7gXpnO5c5lAWpK73vAoF7xewU07cR2wWppDrdxtPuy3QEuYyI3pKUtD18Gf8FlDm2GTcl1a6IKV7O8mmcx6NSHQCc1aR%2B1XaZrXasqbZdZJlhOmAxTiUTSWhcysbZ%2FL7H3Hsf7T3Vy4l5vOOfT82fojZWe8Zl1cPB4d9qBoiAaLnjmkbCsh%2BkjiPAHmMIb3z8zaBQHVbwvZc9XWd0p9%2FYZpW6wnsZ3EEM9AOEVL3I69yj%2B7phoJnmLsywBGKmtqgNILg%2BLVAdu4krB1oZ2UaeuCZGWWKBYu1KWMv%2F4U9uv99s6XMuqgMrHBN%2FuCoe3OSzNhlXcNC9AoUJ89Ces90NJ7XCpNsMM2elckGOqUBTqhZdEFGPQpK8s0IBGwrG9EJElZUg9R2wTjCoylScNRtXro%2BjNzvXyOKRFWSkUuVT%2ByBpzECp58070eKjs8L8Cp9qWUKdUOCIkQJlqjCrRSTjsEIpQysAzTNyFAZn0wwWgUcAs85eTWgWEYS55RL2pAx8zkhY5wGmWfxbQj%2FgIXQHDpbLxWtDUNlpWJqwCl6WUftOD7hqii9LTcZgNa6zTTz4pq7&X-Amz-Signature=6f78dec0acfd945210e2db8e9e31464cf7cfbb7da8a6bd9754cfcc2ee745f1aa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

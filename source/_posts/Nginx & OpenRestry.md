---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNRONTI4%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQCzjB8a9umrMest%2FwX%2BXWWU4hEJg2OAUaCXQ9Fd2hBo5wIhAIUyWgZ7VoeOraP111y%2Fhvar4mzinctxTM4k4dKmVjjcKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRM3tmIMm99PQouOIq3AOM58IyY0x%2Be8JdC%2BA3u7yIBtei3uGe8tjAHSEdbLzKUFb0mpY4YpaWWOS6U%2FFPWqatRT9aSR2GfvBu4dnf5XWs0yDs7a1OaKCuyfav9QGwpNgXfz8%2FbKNVvVjNl70%2BdUvHrmDJQ%2BDHlKxzhUdPDrq%2BWbDyF8rkootOnUgHnHrBFBstAdAi%2F7QQrmUv5fy86y4WCNO%2FOw0bS9s3rUJ2geJvRWiEzNCA4gLuAF7EymBzFjeyhzCLvakM0sKBg4aF0FoEEdqM3QB6TfLynartSoQLtnjdhnt0ts%2FM67%2FMPZMXnIhEMsh0nAmO%2BrVWQSAR1%2BWW9F9iBRpH3ZO4b5dJo26LzU0hDmssWYzXsXvPJI9Ggfx12znwpLiQnTC9QrVSEnnyvuuzNQ8VbJ%2FRsmMju3l1cpwkewVkc94b%2BIXnqvZ%2FZnHejGaIdlknIpgRjfZt3HM%2BSaGwHDD8%2Fx7yll%2BP2ZeQL%2B8JDQ3s%2FxTTST7Eg2leuxFb2CZiJzKlmWDC9ZlXQq5r99%2F85AVlAJ1DVRg%2FSCK%2Be6SkuE%2F9lsd9SZz4TFRdYS%2BHnhvDmDehXzOGMhANDQtkfhmGengi6komB%2Be6y8YddDsB1UA1NkiZS7eajiTi8VB4LmYIv6tAHnJYkDCJt%2FTIBjqkAQbvJQaXiKn6LH6aRHZ8qco%2Fsy5UiQDUkD6CJM78Qaie5jyquWKnff9E9%2Bxzuocj%2FqaKT2dp6obZ0VHPQMiRpSWLn9xxyD33Un9fFOIiqzdsP1NDlV85cMxcAVDZi0zC5otqu1j%2FGEcY%2BM4OSZiqirKKzt3BP43PWpxCdueTrjXQmt4Xfz5WUJFLopcer0sTiMeJG94VqtjQl8X67Me4OhsQpYyb&X-Amz-Signature=3787bab4c4e2f01bfed94e8798c8984e9cf009ac800a010d6d3f345707b85f1d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

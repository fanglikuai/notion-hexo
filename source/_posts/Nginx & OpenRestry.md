---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667RHBEVX4%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCIQD%2FUxjuXU%2BA7SkEcct8n%2Fqnd0cPIZ1doOglF0%2FuThRlQgIgEtEg8N16Gv68CvWQYGlS1V8PUStghYclWgoZqG7QyrYqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA1zyadwdPKM3F7PnSrcA9pZkyu4X799CIWV1tMh4jxWnTG%2F02xqgUQwkQHtPXpv5vgzBl%2BvApUhxEcPhQmNLlFpcc%2B3OA%2FxuhrXeQdCa%2B78DYtMDQ9FlGMxBEEzQzfmdQGx60KNkayroyp%2FfTCiTQrk%2BCuCsAQjMJ%2BTEXB%2BNjroobAenJ5sskXOs6pGfXgPRVmG9%2FBjMNGuRbsBCGcp2vul57xXTlxjDtkHRc3QHDG%2Blcwnl9neV7glkbCbUwPK4eZFEd3vY31jaPDK4b64If5iHuSjZYmD1zOtZFDx9cs4rUOy7YNdcMviqneH1pTyxbNbGfhul%2FKF9WKy%2B5GUdNSZRnKTp8pOPlHNLDAd6drclFqiXEFJsSaf7Gt7Y6X%2B5YXfOxUB%2BgPoqQsby3CZWZ9H9EgnMPqpyztSYVWz4O9qGfCrcuLIYVILpEhf0dL5GJ5DUk848NO3zndYmk4JZvbqmARklOCwrT%2BXDnoXLxndsmo3z2TGYn%2FmcdEh%2BtnJ1a3FvemgkaNCmnCkk04l%2Fz2d%2F3N6MzLfPC%2F0Ji7PgH3%2FMIc082SKiMlq4pcILAdjhamkFy3zJmxJ%2BLeogc9HybCF5qzlrq9NWr0tg%2BgLcs%2BNJH2XrZnvyIrce40HJlRgrsGBMLXfXUZT8apfMMfE7cYGOqUBQSokbUFuQHXFfIkO4pbXDQYKSqJJonU0XJu0jXni4GxyCF6qvX1CJd3gaGa1xq4%2B%2BQCjpxlyiI%2FKwq2MH%2BQUrsMfPaXWfIfw0tGVJ4wJl01Dct0LsbokNhfkZYv1tnwslLB6FClfxxh807ttIuune2m%2FMC1YCgp2yQw7AZB7q16SKXhyT1vpIdOlRQXIO0p0zczB9uC65MFj%2FBS02OHmS5IMrsF2&X-Amz-Signature=d259e6b192619bad4aa599fa25134880319561f3e93eac80c2beac09e0548213&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

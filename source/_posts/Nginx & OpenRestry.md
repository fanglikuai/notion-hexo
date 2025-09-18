---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HKUNUME%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCbu3VtJJM6nqTK8xUSnz0N8LMAFYY3oXOHn2BNElIg%2FwIgNbiFuQ1z4%2FGMeSeA9WLGEASMA8hCMXPTSoWl%2F%2FXijQ0qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLVXt9uSM5Uon2ZlrCrcAyp16EMvOc9D6zQhIM%2Fot4YTDk2MLxhGqUqb5BXnpxno3xAp5RbUaLMN4MyQDC9DxNKyFu4MjGAks0BWwa68uC%2B2IrLGVSHcEKkq9lbL%2FnKoXCKHTdEco99k1NgKE2hXGuTdRfmgC1OUowX1Irrym6%2BLi4nkFu%2FQSPLtcu4vpeM17%2BGEDGOPg%2FYoGff4v2oaCpsABEecoxr%2B%2FIqy91%2FfKT0YSMZG7De6USg%2B3dkiJePfVf9XRSVtdIWls0ueLxkGU3Ud%2FS7rKbS7Fiv6YrfYmLskAhM8PpkcKUE5kQiMXvBwjavli8YQefz1Zg2JwF1Olx2LLlkXSaEcCicEdqHzm8x42AG98OuwYi3xLp9d%2Fh01y4kg6dnO4jK0aSOzE5yn4ANMF8aF5XjlighaEYn3B4lYG%2B%2FhK0dGj8CFi%2B1jdT6evvHcKReBX9M8EUBR5xYTb9nUzwBnT0%2B9JS8u1d6SaIjncQesLEPFbdOnklvKVifHRc5ra7YOOSTl6qXQ3rWrCdsU2hGBje4CceAFESU0RYcTxnhiVP3%2F8SOVUAtb%2F3RL7HOVR3jUOvndgvaw2scj4ucJwGEOtUMFcUdCQMr%2BhuhplGqEFbyXZBVaQZXXre5GZ6%2Bo4q7l08RFNi%2FUMLe2rsYGOqUBTfAMQV1mtCYB6bG%2F5x3HBdcr2d5i2E1jxT30eIwg%2FHsimMUf7%2F9hUhGDemuOnQgEgPHmD3oWj5L%2Fzqkd8KkdqAz1KHMd55AXXY3nWddeJb9iSp7WqNyOIOdWQoOt4ccvm2n4tFBDesTBkHsjaxCzoEQb%2BrFxb0loNSxcNR4nq%2BvF%2FuTQAWMnPJsEz%2BpNTLXLvFBuA8pRbKL3EAdl0f4qGDtwAvD9&X-Amz-Signature=e1f5855ede14f1b06ebeef788117b1ae11d8a0f9b5ca5213f1fd8a47e96457a3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q574Q32N%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T080039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJHMEUCIQC4N%2FEacmIZEnx9Ss7tHwNFfc1fZaaXP874mYY4ww2CSAIgQxAK9JqdfPWCbp2iOc8o9FOreqiI6t6NkdmfJdcfO60qiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLLIFl2R3BFwNgmv5SrcA%2BMtgOp4LxseLrhAJlJVy%2BumI2Ttp2QIm%2FGpUTmyZO2itfP9Tzuej9L9KQ0M%2B1Ga6W1zS4npOFsNH80kMZFEtH5DlytGpX%2BOrAGQ4vifgDQRQu86RryxG0gwflHtIYCtXZVuRPXSEqEUao%2FZRYhUqlCDiyEX9Djx81ygbZY9fsehwALxKajLbfEu2G%2F4awSb8E2%2FrdwttBCi%2FaD1QCO5i2uf%2B7Y1US748bSl6eWEGULMpwP9%2Foo7gNXE8%2BSsGyABW3wdivkMUVZZ9VZcaAxgEVMBqQ3BrVxre6VFTIOpnD0XI8ifHv3tDSPwbQcd8w0c%2FLNUUaBBo0r2uA4gjTUtxuPBkmmo0I7ganXg7lYGmEMQjySZiEhycgawD1C%2BArfKFYefjtxuAfP5hoOrmwoBnfktXjyy%2FZ4aB2dc2wA2sFqAaqXJLV2OmHPyX9tKbX0ZYx4U2OB8S2ZcYA7eFzOfDqObIwAhb2BU1hKw%2BNPyLbp4YI8eGvczK%2FyOREQHXOcWvW7CVhia0VCAucQnhrcntC96t1d3rFu8SEHa5u85KeS13Fcg9hMXKCFOlXSuDbEeiulP6BjvzLGTaileZwhRMH7eNPwyVrHOGNh8RQSfQjML2PD3s194aZxC9MU%2FMJyG7sYGOqUB7OxL3uIWuk%2BZlbIN958gmC55FuHax7kul%2BHtInuIG%2F3lysufENumBFM4qG826qycKedSAd%2Fhzb026UCguvnywkSo8cHgrMhBAJFLENNdCJ3Gu8CIsjWLbUiHmGA7gwdpYVU43d6jYYYsQnaAxNhHNSwOxtty2b1R5aMgOX9oG2Z%2BfSkUoNFowFAypWzPDdORmg%2FEbU%2Fbbpb%2BrLa5iINGemEkAhKh&X-Amz-Signature=c5bf969bb41cc6684fb231be09879391fe4193aceace542a3d4b2103a17e9597&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

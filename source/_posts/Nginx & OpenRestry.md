---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXW4EM4X%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDWNcDPAAUy3sXWCUctkM0h56NHEIH2LCqm28r3C9MSuAiAPfJZJFNFt0wgEZ38w0QjL2TRn8MWxW8kW4D76tQL8lSr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMHrqFdpj1W8%2FQpES5KtwDThj5Oy6w5KIQwT7zktTWVEoRYOSNXPiKJ1Ziwp4L8XOpAoezVgGiWeNqwEukNIo2FzuU9IQ23eWUhKZ8FuibrVDvaUZtXEfR%2FmKPU9Y7Y1R9j6XgWO4qTzD8Dd6X3AGijumAVpF6hPhChanAlIfU86KGEf%2FCixtfpXUstd%2FBd7%2Fcj3ze5HxK561pd4M2pIwXo40LnXYX9Glero%2FZNLDRkEaz%2F%2FZh0d%2B5lHcUgr0wZIs2DbboJkwqRU8uMhMhGPicguDe3ypgMQHIbc9JgLPQwbfLTrVz1O7zLYezItCLR5QhlcPksk8ky3vyM3OH0PIEDt7pm5Uik48FePyPURiI3xmjXaiZG4peIbUjVCXCl%2FqQIO2Vt4dl2cUBfAvM88qC5wyQulx1bYgDrHiUnHHfgnSZ%2BHdWr7v1071%2Bksg8xDfS3si5kYFwIgBz9uviB3ujYLIz3L%2Ff10V%2Fjr9jCvV3M7LVxjp%2BeCpG3KDrXXI16f9SkiIO9kTK0y%2Fr%2FDFOdv1LMHYfiJCVUE0bNGGPv3NevOHvbAlJf6paK8kMxFzjy7PaEsOmw0omlcCSvm7niSk9I0SUsJDPLuLC07xhcdEvX07hNtIt4AZKByexCJYh9Ddt0Hau7VL0uXAWBCown5rtxwY6pgGA6e6quDo56gpyGvFZpbX5X47f7Fz9DKHhWIol0K7PgHaREIRSM81M%2FuFf%2BBLuXxnSagMKGxAa9luoGeNXzclirCDt0HqTu8iOljwqDv5MV5LWUACr21tDwsAyKvqdpJRoEH8R2uko9XMp69kyblTeHjjFbLR3o7VYyCXcK0kdDDW5br9cdkYi59Ft9WaLVy1Z7sa%2FeXKnRVguoJYAOgXLXz9pr1BZ&X-Amz-Signature=bc83668bdb2243570f5814a066160bf86e86f577626061ea648473990dee664a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

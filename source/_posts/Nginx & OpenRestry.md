---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNDVUWOR%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T040044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQCEzF94%2BbJ1CZbHs2uecEDLB%2BvhEUQ%2BeiepnFgXBaIrwQIgK2PnGuBn48KSrDh1z2aI3UQimlafJaoFAncw10MWpnkq%2FwMIJBAAGgw2Mzc0MjMxODM4MDUiDIDsifyxpVQe4UG4iircA8ZdTWpJexKfbeKRTVPqQxK%2Be5AtzrGHkAjETSqm61FG6PoSYJFIMsQ9uLmIGh%2BtCUdYLMwlIG%2Bjfu652mAvgoOBPhTbQ5AOCA5ruyqfgQN0PdxRlPmapakFk0%2F0ZKe9Iw6a6AtoJ%2BxCfemER3Qg1KP0d7ZH3kiAF6xVeSoZvMBzDsud4E%2BQC%2Bej8Bvfr1%2FlHEyU9a4DxrUULJcpNPXQz5oVHmJmupLrgemf%2BdEXwulQ%2BlhiAQezGcHgv1HB7gdVwRwHW%2BeJeIRi77K2Svem5tCtme2xOWy%2BhhAiu9ntc%2FqinMXDZoSbhuXxHN0o4pSWNxjb%2Fg8ohT%2FPwLH3eOX%2FhgbsyNz2t0q0UsZlRHEI328kIpToeJnT4dzEWIQCQ5Ih3KVbxQi6Yz%2Ful31QoU%2FZsP2zKc3wQiUU%2FkWtS1r2rL8hrWog%2BLIVQRt0SnE26hkQgwxwBNSxDIPkqT4EL3fIgkVOQ1FjV240Bru%2FDng8p%2FfQbGaQKFWU6LO6TcZQ0bY%2BT5Q7Q%2B8DDNwh3HlvGqJjZi51EHKV1Rwh0YSLEBBmJWlDDo%2F2b5wzQnBrjkQJov%2BquhJmzQ66asdw5G8NR7XZ9%2BFRD5gXojq3IXXzHWfbD%2FBXcOS7gBoYuWzKKV00MJ%2B6rMcGOqUBqQPUC1%2FmmhA2m2K%2FTIJ%2FI621d34zW3oj9gyZ4VMsIFWh3m%2FSXWg6SgLK%2BaHOtJ1K2h4WiQ6m6lN5peTRVwb3kuLKTsSrrM49dFTWKuEv8z7SQWOvtCvGKqFmdGFIGKydWbhlOWWaQkEngjsY%2Bir%2FqNZJ%2BnQKn%2BCZ7pg1KJJPLLciW%2BbSYQAg8kjb7lShJyTvoBEflsxu1U9Vwh2OwSBmiaC8tx1I&X-Amz-Signature=406aa0a04c7a5db6b3fc57f32bb59b3c0c571e9ef24c2901e22950b83fe2b7a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

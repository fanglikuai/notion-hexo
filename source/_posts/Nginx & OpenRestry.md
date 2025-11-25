---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJOM65SU%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDr4rNWUln5%2BjhrUN8duKhAejUfQeE76XVTjeS38NqraAiEAk9bzAwMzunHS30CApIuzWR8tF3zBYwqvPFI3J0To03wq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDCL0GYSKOiIaCAWNXCrcA%2BhMqXY5hCF%2BrwBtk%2F%2Fga%2BJ8dshfGLeb25S8TFilhr72bJO5AhDgubUU8tvjAsUvRPnqpuF9miVlAnVGA6tA6BASS7qTSQTNENR8RnDYsuRz1GSgCp6KvtbwbWFmaJ3%2FsSECp5UHlVCmjmXKlQ6NkoxRgKi37InC5eF4PTXlDI1AD90RESem5jiD6i3dbzrSr8mHNR%2Bk6QC7AgPhYn9yxFLhtB7EdHHcOGoAUcILYM8EnFQIQV1Wi75w%2FBhWCMO4yTmwzq6Q8XGBN5SQKRPsTlzw1cdN%2BLcZ5pY642rIERQ7elsn%2BdHYwTo7Oz0THps1E0zGGQ5EURfqysOniNtEg2JsApoZyCr7hw5xE%2By1W8maYB%2FnXfuVTfQqvwcA8epBto73I0n1F6r0O%2BfpyKeNHvZ1wD0dq3MgK8BHmYHAX4ZrQyRcu%2F4dDONUoB%2FYCAZDRDrkRQQmjYmpKYSdx7igQ0WVzndKzojZBjPVfCtV2VkHsbQnupe8T3uj4e%2B2JtV8VIrbwHQ2oPg1OrP8QJ8LYPI5ybCYVXlfwHsqOBEkS%2B%2BsCKG6BprPLVNi1%2BT3yVbF5xxns9GcEqLdDZB%2BxdvfHwBMTCrL4ZRTz1Ct9OlcKOr3SMOCNjURv%2B8O0r%2FwMJvXl8kGOqUBsWbwgTnBIr93GcGHmFTfm%2Bnmx4Oh06Hzif0IHmg2hmL2tQxpmERG5FmwnnD5po9wvhgOQ9PmXEitcYqgzRG5y%2BXNmQ75HDKKK4%2F%2BATJCUR7KWxudUvkFRXgyGho%2Fig6P%2FZH26mCCIMdTtVQS8RLtP5xYujGmdF1CbmXnKL3AJVeX3Rv3M3h%2F%2BJmsixKHyqtOfxGAIkHXywiOVfCTpBMj2oGgn985&X-Amz-Signature=77e39eb89d3d886f0c13cd810e4898c78028e1e92f8fab90bc043ddbec073699&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

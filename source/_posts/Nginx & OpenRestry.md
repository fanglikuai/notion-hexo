---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQL5FDMW%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJIMEYCIQDemhQnmDe4v4AmeOywA2h22WWtP2wOTt3%2BWo1jGqsZyAIhALMu%2BtF9XgxgZJSrWPGIY%2Blh9Oh7fk7nJplWLYtAan%2BOKv8DCCEQABoMNjM3NDIzMTgzODA1IgxYIHxC5nl%2Bed2zyMUq3AN2Ln5Rk3xlNm0ZelhaTZhVazEmn82BUdRo6M0bgThQ1E6Yz8wKiWf%2BPSiTW165h95PqQsC1qFE%2BvmvtAHILyuEm%2FJ1cGDyRkoTKfwHsfattVN1BJvfP53PvaEYZ3M4l8Kjv8prfllV6RCpljGoFdkfbDGuVwEoFIhtpBnxUrp9dLgzMtkMerF0RTnVulLPnnmLP9rEZcBqjC7PQPSUtsu7FQn9T1yja%2BoPR6eghdyYwoKZ%2BqcCHKAS2LHdTIsLz%2FsJLHkeXQpAJrWduIql%2FsF%2BIPiV7V9k4s%2BqK%2BpWaqKfDgqTXUcMG67H3QQ%2BBJfh%2FnNJmRsybNOo7obpsy7FlvjRDGOTeM95wGI4xNtHoCdYZeu58JvSYpp62uRyxMW2H32VOF%2FPJLU0dTmPtjvFdj4BTcTnbCxRhUt43xzLZHTQLMoMSIpWxfCfdcSbglJIsO18guSiVk8ljxil9nwacquvUAKcQkdm3RxlbIcfms9biv2wPpQMHYlYO%2BeDGPaVkerGBVWoJF1xh3gfo9Q4mpvNDk7cL2PEz7P5SsnZ3ejUBvlj3yzagh%2BkR8u6YG%2BhnX1vaDRJS3X30ipl1TTtmXrWLie2CL%2Bo%2FnM64YtFC7p%2Bhf4Oxb191Z%2FdEwcbgTD2tuDHBjqkAX2yUIxo7wMmbELu%2B36L8i9ey5lgILj5HRwRBoQX0Pz7%2FbFOzFLMLInAIO0wGVLGIBDMsT%2F1qwlr%2BGXA98JJCGUTQUUtQi5do5UsWXlXva6sKpvOBSRyT10SOLOgKKhVPCPwBxe2VABt%2FFDRLv3WaRs8dqAFf48phZgi6YeBAEW67VDhwgVHZLrAx1ucf%2BDl9zItDhGbALhtoK8FWw7XFnUin9TF&X-Amz-Signature=6c602edebb9b5365df1a40c75dda7b4c57a8617c68b5d0cd01ca48aeb5366f0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

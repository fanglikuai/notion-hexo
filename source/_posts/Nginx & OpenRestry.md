---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDGBXGG3%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJGMEQCIAdd94TbXSBiglM6d7C%2FmY9vs6RJTvouwIxhtpHQHL%2BjAiBaSMs8X7S2NHvAo5o%2FQr7fAlmuBsau1xT4HQomIEIIqSqIBAiy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCRksZ%2BWvKy5KZoh3KtwDG1xXxxc61GY4uIvsRvcty5mYNy537IaCmmEqZVjJbL4y3ZPuri3DS6w11MTWyp6FTvGLYaEEq1vXaDLU1IlraeypkooccdR1FkE5LYl6gpyU%2BacOwnU%2FuVkB17beQ9IJ0W5WzK0Mcrp7aM%2FIvkwbQ7o8y3TMesqhljrq2OqhCtVTh%2BmK6inXioCGyGAW6BkSzakIXDk7Y%2FOJwaek7HjUzque8l%2FaFUBfYBUGZmZElZ3NsrDIyiGvgk7Fcoc2z%2BfPtHh39pdjYFZ2%2FEw27Sg07iMz3gOy%2FJgH22PvPmW6Vd5rc14mwS3sKNp0JIpTdzNzDMFQvDKiez0NVH3EqRDDgHK4mAYpzYArLEHHEVvvTBNbNnN01DHtqJ3XC2ldO%2Ba9Sd9yF57jRS%2BMOiq2ZzFCBJoiOcC1Vpbbvy6rcxD4J5P3NYlFr7ScWlhMwUtQXv9oNP7SFKdSzx8IPlKNm%2FJycMNjtEquEl4kZgKcWXeHQMO0Grp6zA2hy%2Brfqoyuk9%2FLZh6ntByEiwZvaA2byMVklWs8blnl%2BQkaJHdr%2FdhJVOEcPIdndivk1SytBjKZIHnSzbxyZ8xvITBhwrHP6ie2W2exZ9iEh2yclm3bDD0bBm9FHm6brf5uYnUyIOcwq7%2FLxwY6pgEorSd9UOELMDqm%2FIStUpYOypnsBewHvWiIHyOHirxrUq0vpyL9Nzn9fWXVm4ahTBiu2n7r%2FEuDLKxGw3h0JBYDYKPur9Rx7tE4heLMLYtuT4mj1qrf1gW%2FbeK4lWir1qOmuLM5FK%2Flz%2FivfpTXCSVhNnbPWZpCf92hv%2BHMpvkaXPQZGSz%2Fd%2BKHilUiau%2B%2FZBSgup9tHdwk74%2FnWbYJNez%2FELLwXgA3&X-Amz-Signature=01a3ffdb1ab6d69e4d68fa8c45fbfbe9178b08c14dcdb8d38adb31d09be76f92&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

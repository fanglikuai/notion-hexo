---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JRYWWOR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIHOhbPQ1dQ4JpyxJG7vIB1s6JBcRYIa%2BlP%2B4VRljj3bXAiEAv2gr1BKbOPo5%2BaizadYur9YX0gmsgmKd8yq8QRQiWCQqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBiiqd40hw3cVoVfPyrcAxo%2FhsFxKS1U3ZLU8nuFN7qX92Ya6Befuj4Fa0jjNWnBDWtSpS8YiqEYTy1DIF5vN9z531m39brFu85G6MNJlsiXfbKiDsI%2FzXJgf9HD%2FzlJgXy8Qetsa99dkXJopL7i3Yw6mnaM%2BIzXpSrt7SrmzYxUTUsjvk85WlOI2HW%2BUXVieW%2FwdNUBUo5wSKHgx2IrEopl33ieT6RjQN42rHiKeQZuttNUCoV7BBFk1vHm%2BAN8sksilh02u%2BuiAUkaHRtL%2BJmw%2BpcPvBrzAG1VwrAH2bgKlue56%2F35xP%2BMxPrdoRDoqArFKVhWc58oJTYIOWNF4QWfJYOlXW5UlZJKv7venBXm8BYkNy0ara0J%2FjRx4REMBjHio8FMyJGMDwV4WQPZoakaH6omyrKSw3ejU3NKypisaWhpvkycivwvhxzSmuEeV5uBAjmwAH%2B4Mb7YoCKhVEmGVFSyGjw8ruRxD1hZFDf9MWmMRIupHO1p8jV24QHommXRxMFGTrFdOvVIbkaQmXk8LVn6XZPJJLh6jKtWw1syiFPhSRMgDRK612OSd7I%2BI964vcdm6XZckC%2FsLDtOyQUtMzZ0GdtQby%2BlPhMTwtXvgT%2FrrNFKX9NDOxSVYOkiohEt1S%2F7wmjB88cbMIr2kccGOqUBAlIJNP6hz%2BatKrHaAf6kpjimIfnZIybFgU3hFpb5Zci%2Bol5d6FvRpRzXzr5nCuodflz4CEMGlD0qDA%2BX9VEVjCI68DoaIbaT0KL9nzylrKnL3vt%2FOCxNQaK3WMu39vjvzNGNZYWIP8OYeqRr8BlyNQefvfjQOBL34P9tmuyfBHnh%2FFg7a%2FnL5ZkAruM89dWq6N1JSvR%2F4vlWDa7fBMQZGU1Ibyfk&X-Amz-Signature=d3cc7da34648958be210fc2ebbd1e478fb70cabe2adf602a29e087a435b6b7ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

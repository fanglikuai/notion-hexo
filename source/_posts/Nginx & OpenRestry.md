---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVIY3SXV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQCqyi34dchLgUs2t2tQKsdJs0k%2FR93YjnVhOQIWsnCmEAIgc7QzoS4G2vjzGRcJTVMmIG%2F2D9xFXLJwbuYOZ9kgqH4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDANEN5Up3q9D4SM%2FVircAy3OxsuM3agtrHYeomRQObk2aasqx%2B3Z8SUPKux0PxajscL%2FGkRfZbkCb6DYpp4s20aHOUYj6ZuIXPVsxxl4asXkAy1gCVVa%2BeXijt7nYFEjPVEeQU5El4CEjw%2B6oyiO%2B%2Fa0p7fRr0eGo7cyIL9kTKBmmg2sBYuCLb09PvZnmqskiL48VFUJ74qLBw8i5Vxzh0H6lcc%2BLhM%2B9WwMiEzL0y30MuqS8B0y1nd%2Brd7lG%2Bzfalh0GzE2ugVp3P%2BuzQbRTsg%2BRjYG%2FLBw1uwm0AzEGq0ilNfCGSEaWJnVyZlhv1Ajyxh1DASTep5wuzEiU7n2R%2BJTo8hRUkbtAN%2FSz5nbmgtvO1iOP89acxUmwHd5YcYDR8zdkty7NmmHGEtcLGTVocPGqRjPI%2FCUeJ21FFrwsJRIbvR70KkGyvg4zVdaBk9xlBC0BitNM6xdUm5eeXs%2FikQGHuiD%2FBL8yRd%2FbjqtDos7oimglqYkyPAQANCvWf1k4M%2FkubfLlcORJHCyK32aN2uFxia1PSEUCxCo%2Fd1aC0RPqH9RpbquELL6CMaoigRJhAfX4NjHCdfV2sC9WF%2BHUOFM775VwL6KA5F9gGQHXjQaFtLPU3YtCRzIlK7E%2FKh3j87GtPVRAentsvGTML7nuMYGOqUBANtsNoGxFFcaOX9sTu3WGt%2BA%2F7BXxtkwgbu24Um19NkMZWo1SQ6zNJfhHi%2F3LBBeEBPRfzdJfdP0W3wkbF9R%2FBZRgM8zS21LwedzgEJ4w5VIi92%2FusegfsSpw82v9FGvJmTMzKWMImjJIPn3S%2BM4Wnei1UycYXzPcGeafBEbkH2S0%2BugrEFuF22wABqm3ugyO2AB3C0Y1CyvoMjQFWv1eN8LYsau&X-Amz-Signature=53aeb6844f6bec7b832f3a3b0487edb96a91e6c401da361463fb001094734c47&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

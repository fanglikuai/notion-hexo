---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OLJ74EA%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T050050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJGMEQCIDdCzzBO2ib3BP2%2FfoAvZ1fXEuP55iLHG1smz67JoBdwAiAUTkafr0uicsaSTvCXI073EijOSsJ1XxJ0mFck9CPBfCqIBAie%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMJU6fefmAVMjMr7nxKtwDEgrUn2w4DSvvCwMf7E1H4OqZCGB8KmwaSOSOzdfym0Wbae24CI0R%2Brjy3wSARkZsvwY2VRdfkj%2BfHkKAIFMNSeqLbUBLFn3zwFBKRHzS%2FBdskI0GeNIYtJLGO%2Bb0jG2r5wnAJB1fAci07V8uv5HBC5ddGZ5el5jlp1eKcJuDTv2qW8K%2FkS%2Bbk7QdcWMPUzHrBdeRP7f3D%2B5CMIZq81Z216%2BGhgqg8jVs%2BZvA68nAZRm5MMp8RwX%2FC9SO5qL1G%2Bj32E0the%2F5%2B6namp2prHVzPziAmQ%2F9vLggD4ioiVKS3Y7zLzk0e332lgHR3UlGV30C1Pd7oxbgVJrygrlKdPb%2BWN7N%2FVJ4rhYr4JJ2jSpu7qX%2By0dkMCyQH2EB%2BlsngGuuOhPZv4%2FVS9IoIvvYuhe%2BzgpwkYYhdvvO%2BWd6GZrzxyuNzjzmYfB0hxjhjbthgR6vctp3vhuOFBDvivSAooYIkyzKcESiJ00rHRQ2fYoVAthsh6%2B%2FUcLc4lHRE%2B0HMC%2BeLLhDpbRwkCWtCaooVKi%2BqdSdvXT2yOE0MlH6vFmtriUauF%2FouWHdYaDpdT7IqkwxnChz7yRtzQFBI%2B6HLyNsX5lM6%2BG1Isa5Isf9Rl7PcvMJJHTP%2BCMIZw6w4HcwpNzdxgY6pgFuLBWqN5wTpqaZuBIVEvkygGuwNdUtq8D3fnB5msR%2F67HX52uimOUPEUBQESy7L3Mecjf5LxZXl6GCOtbctZOEbw4l593aJVZdXU9UF2oPbA352zzjlEtkNenOVzs%2BPTN3Nv6WusWQLxJ0xZ%2Fe33ZTwHfzfpmpeWfzF2l32QtJFrmdkbOmATPw9FmOniAGNFpYxw7IWf%2FJpeWSDJRyod2%2FkXbcV1p1&X-Amz-Signature=e53e24c276033e1055590e77d5323d87f4f6e7fefd835c365fe729dea6bdb5cc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

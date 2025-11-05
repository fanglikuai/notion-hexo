---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJWA2AYD%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T150104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAQ8747xHI31sSyeaeF6Is1vOSjJWFnDNU2hDK1NDKiEAiBkhxyZF4FcBPj3LKW866NeaC%2BgtInYBjWMR4AH1957VCqIBAiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMtjYGQQnza2ksIFlUKtwDg7eASpUnrDQz%2B82Bu%2B2JkhxqS%2F4wBH%2FpF3kAjkcmBtAF%2FqjJmDRa6TC0QiWEm%2FnFEQef3mEWF1rQ3Jk8Ae1%2FqhdoEK1ow0JDqzVsaENQsd1coRurBLCjCTUw6oFidnKglrN7Gq%2B%2FU9PMbWE3sYNzIJB1SLZU68HNR7HEtul1R9JyeZ9lWkvVs%2F%2FkmDvgFec7skj4lb6VlzEMkWaNJrUoJC7VGBgVDwejYq1ytdwhxTN54miN2SNwZaYDQSVM8I7aYpHDgskUDiNIj657nkVfEVheROAYttVY6whyzUHV9FOi76zf4ncRsjK2zBEAj0jj4Q2pNpaoqPxzg%2BUUBZXIIXrtjMvo%2FuTwrxi8El8TbEdQ7jRtRpef%2FXwscqtbM1kRbTwQ%2BP72JAdVMwBEjQvy4U790TMmgaY2FQOubCPmOseH0stuqf2K3nB%2F9iHASPkTDNo%2FkeLc00hIjjhdhFHVstFO8DkP1mvzZ74TOZIIpnTDOyFDHywI4qHN%2FNGbHnnRFskzrFxBoWzFuwJD9nrzHIu2yjj7qEgniW2J6KVeVwcnn4IOkrH9vU43ARHJzRwvkU9x%2BjYopntgXwC%2BFhUc%2FwJ7WKwMaojAC2yuz2SkMS9lcl2gT7E1hh7qcm4wsrqtyAY6pgHtfmfGMw4QCxnXyAEircUyziNRsFko84jLtFJG1MDK05fPHZCSe0EqcI0c8RwQppIQ2I4Bu%2BB%2BDfZXpStYqKZl6xxSSK2BE1Ir5Rd9HpnyBLvrn8P3K5Io51l6mWnV1iH11nGGokjL0rQ8CJAvKF2UPc5P8Cj3JExSBCAkzBhQiOznkgXC0otWe0CQ2rsE7DFyE3IEd9omkvA6sdTHwQqMaTjDEj60&X-Amz-Signature=6be05086b0d8f7fc4356348bdf3d3a829114f476b6ffdb837c6a043d83f12cd5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

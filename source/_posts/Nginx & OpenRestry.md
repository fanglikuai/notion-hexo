---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM56SZN6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCICCRQywGn33J3ZCF5SyvCgQPhD4BJoH3Nvm1tZ%2BVMe7tAiEA7TTyGUhWU%2FyZQzHNtJkqxoZaWKD%2Bm5O9Y8dmQw499wMqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCTHg8yRexoPLWHUxSrcAxrXKOpIMa5OMhgd2LcLoyNw%2FUJhUW4GAieI%2BBeg6L8FHf61T85OGJnMAlnivzXbdWVTTPZ9iTaLI9ZjDGgg9SJxfPDBlpKD29P%2BcLvgcicJfEwJbopAgEFFSH4bsn635qGWlPcWJXNXQQ8I7EiatEAZxEUWB4%2FHFoCNbAYrWZJpVm3LLFjPxHWw2RvcUwmktIe3CBZlFTbrMnEXGRgAuDDIVZWgstoq%2BmNKPsHDtyTtaoMVTt0PS0x8cutTWn%2Bnld2kEaiS0fUesIaKI6Sb%2Fn1rfntbA0wXevHQODGJF56Ho8ugtR30OgSSPQ7fcnGHiULw%2FYaRz6qg7ae7cSXf4zwM58B%2FKwXh%2FWt5SDow0LpZ4zBTi9KcvrruNyU%2BaRev%2Fzc%2FTNq20fOqLN7xxOosN4sknDFYqxAbbNPUY%2FHqLCLdiCA2i1a50Kit52kiIxWMthwlAf4ezLNLpwqqy3q0i9zTYF%2FujnwaxpWVfworg3oxwJaJSSxZQd7AnwI%2FW6A4vWQhv2WJhl6g8n0QyDucNSzcJ5gQuomM37n%2FiCga%2Fw1qWdqiBZVsYtu9J8ezygklIujUI1PBuD0chsvdFOgac8gNvs4pcLidbS4EMBE0iMPAqDJ13oL%2FUeqA2Y0fMP%2Fbp8YGOqUBZnwxdBNmckSE3dazoH2r6Kkx%2BGfpWX83PnFTxLuwXdCWkwjXbwblXaH%2B4ALtZCSJ6i0Nlpm%2BpM5BATsAfeJfJICVmlNmQeZJw5XWOBCBwvqQeAjYf2n1RXzEE9YimSXmLaMuzbut5CT2bvpBzOcmG6GGL3F5TmaljtfqkPhkKeifulycE2os7DcL8WBOymIUFppCHz9uhxtBEDn52kSJl1oY2WYP&X-Amz-Signature=7aa39e0204b33545220f42a92db5257e7e09dbd32b9eeb0f0ab78f3fe1eadaf7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

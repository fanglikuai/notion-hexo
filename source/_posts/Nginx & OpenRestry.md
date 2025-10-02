---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Y4QWTS6%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T070052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqRQHs%2BJigNVpSOzt8iA%2Bn8NKQoQpequ7N%2BRAbSy6gLAiBP158LfI04127Mt4O72d3hB0TabfwK%2BpjY%2FJFoKIP6tSr%2FAwgnEAAaDDYzNzQyMzE4MzgwNSIMufgMDwCB0%2FVJXwkSKtwDx8NAEWwvU05GkNFUAT%2F1oGKmmNGeArSmgaZ10YUaoDiukQPiVWECR6ppBQhoVza2MioQxpnQO%2BYMpjbCmHfPAt1E86XsWJ5xtvJjfe6qkAMBozkgMsTWYFcpQrFj60Hg82GFcQM05qATu%2FkFDx8%2FmeuTxh7fthQfV8arZTpX2xVup86QckcXQRULV0H%2F9Z5r%2BISOJRgyIQPS6N%2FO3cYF42G0U79hIbVkTvUWJ3aXxOmGNKCy4wAAbYE3t5BP6YO8XSGJJn4ofhBEMEmkx65mRh6BEJuKt%2FIoh%2FCzyv4zzjBac1fEDARZqgrb%2F2HobKFwfUEftZxvZs69mY0e3UwuCKkW5DNjq6iTlAowVOcvWCMlgAn29UoyAJBsTHp8ai8Xr92xehLWuIa%2BiLDKA67dLbmgOCgWHvfWs0ViqfyIDiCQ32G04OG42qgWrU2NU%2B6buSgj%2FE14GatnyQwwktaHI9WxB1DDhWn3eSvCuxrVVB6c6tNERHhUUNRJfMSb7txnEXL4w%2BIboenDxdfUUyHO3xCyAeCFJAlAJ%2BWJyvlThro7c1GUC5xbqdycpNCu4AUQoF92gn8xsDGF0MoLSR2Gc5wxqgdkByZQa8tJRLaX16RP0eDjeOZyA0mnl%2F8woKP4xgY6pgGgqF7%2FqfwKwk696lfzJ0to2nC55A%2FdGY4HKe0EXUNft7cLoAaT2Y9l5T3iQbb9WBNe0lMu1EzkT9RAv56lzJ7u4maCnTW2TJXo0Cgu8WtTFJTOLLJ4bjwdTrmwDpd7xKlyGDoSa7DSpqwnrxgQBdeE4IJ9k94BV3fkN3H8NcJQPqks9Xx5vgXbATNnGl1OM0ZueS%2Fw51F8aYbNmzXARpyhqCfLV2K2&X-Amz-Signature=c7b046a08582189e3a7b7cf2b35d53cad61f42f2b869c29bc0701ab3013eab18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

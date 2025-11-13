---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEBHQYJD%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T140107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDfvMR5C0BvS5U0r4KL18UHYkLo67sDOAESSLDWCBiDNgIhAPJ%2Bmddg9gSIP4G3RYy8XZ9c%2BCzPq3v8fbnjrq%2BVPO2PKv8DCE8QABoMNjM3NDIzMTgzODA1IgycMiiFDDjJ9QwD3%2FEq3APxCbe6bsDVCotr8a%2FbNUqm%2BDByKeP%2BHYCtrqVn2GBmYyqv2M0Siza08KYtxYSD2cTuiR7kn%2BgO4eKUvgP4Z318WdAvOuRxFfPYmnttXRWKtBUmQQtahwVYEdX4LogV6zJmkdjhyxU5rj%2FyqN4ywbVV3aWpZ6zmhv0YptFTevkgYL4tRU0oUlFOO4y5oTI%2BNXRTVr6LPAtYp%2FDSE1i%2BdRsc5PLWTrUAa7yAxJ2mzOueYb382Gv2b%2Fia59q87rQF3GDSQr0a%2Bp1ABIKpOQ2cxe3OG5GRdZxm6Ulp5LJXJd%2F2AnT8UTdidE6a%2BKwY50CeTq0VuBbNpE9nNqyJbxYSwMhucSVig0HgJjgNJpN8ICLeXQd7fv2d5F3OhpPpcaJGat68daBgBaqdPtzvVsBeSIeiNGSn77rsDc9LzTCRX13%2F7ExGnk7PsXwVFDPTJzM1NZrVWRCRecR1263ro3cD70QkmrJ1QSm1rTQCi3Jr8eZ741XMEhmR4lX%2FMDx7iS65kZLaJ%2BxqOIDIXLtdNX%2FIi9Lgy7Id0Ls1PAjndrb1Sdw9jmqOmNzUlhlEOMOXMnWm6%2BC8rP%2FaJDfyiqPU310O137XHJ2Zm%2FkdL25BLTg7DMhbQnkRYMqyVZMCuB%2BjfTD%2FwtfIBjqkASMjUQn%2B6Z4iYTbmisJ%2FFOgDCIOSDTo0OlBHDNGPQG09Hd6Yc2lgdBnMQ0sUHF0WA5MZRyRRytRqtmHgNjkFpVcm4rFvtzGeYJ2wLVHEK%2Bpd3pZjNJg7F8U1f1z%2BwOMmWFtNOP3wYQXxvHRZ2AZFR038pnAKir4IOFzS48IziXNyb6J1MU43ivcame01XpVPqsYXLNhMK9RjJfj4HCDhQRP189ZH&X-Amz-Signature=10e64df68488fbcdecf0364f3096d703f6360aba353714dd8d0cff90790d3694&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

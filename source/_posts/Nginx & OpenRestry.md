---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UGQUK3M4%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEsaCXVzLXdlc3QtMiJIMEYCIQCqkQfxjMBaB%2B%2F4CZvp%2FvzxNHuGXajc%2BMg9oJE7Xub60gIhAKPG2H0s7bG%2FgxTUHogeT%2FzJl0RNf0aBOljaem7dtqBkKv8DCBQQABoMNjM3NDIzMTgzODA1IgxhO9UhfDvEpG1VoNsq3AP%2BLaVvfKhQtgbeNxVsvyw4CmXDriDtEygYAKAydG7yhXKO8AbEuE0xnFRUSzhgQQH2lvb6fwkTEFdvSgMiEv84YUSkqs0DYceybYtz0fAuDWN%2F9msOPA585bTbDmHcbAKN69DPdG81baMpEDQRHyBN4%2BnBaxDZ3XEr9xlwBo4wp8XURUS0Vo%2F%2BLa2HfloKaC6PioxnxMVjf79otLrpulPVun1xZj6xPb8yDX60SoMGRyEBs9Py7yy4P9ZzCxDrxIWdTJWj9WbrTviamOnR52sfpjEVCl916ZwoVxjJIG23bju%2BNacXbj5SwbVlKkNaiK2MiwN9lzTC20UL0HZMib3rwiV2wK2CzXZvJuUGbtBboGeNpIItat2jn9aO2o1cjmSwGRdlZG9DuYABGcdKbkQStrr3axxHUVrrbiXlbo6itgolSFNYR58MNO4GTUKBz6F1HmHOF1oLf7HvKOpIiIHxDllsmg7pwo7AIOmVOkaqwMAnBt6LWg3QRv2RDU2TWW6785Y8EwdRO1UwvLCB69zkzQp3JfcQNKYcBAwFvmf5RFzh1oS02b0N67WuUzQzC67mW9oLkot1ahMpjt1g3TpEGTa%2BFm9rCp2WWJpWhh%2B7%2FhQ8tnxs93EewzQbOjCh6YLJBjqkAS9FeRiQbKBS9MDpmgQk96LZx9aH44NCaJN3AxfQwJhpyGLi6Z9fr0XagVD34HA71FvltZJysrqik0I1FkCM2okvPxdG3e2EIo6%2BFsNmBTdtEEDX%2FCx5H%2BQCFl7FiFccUeAQSAvkby%2FyE8etuxjJmluTZ7TPmC4ZoW6Bd%2BMZNs110KY3Y5hZ5WmtbrqEkWb1DUHlwcZQL1QHlkY%2FGEiGW%2BR1M3Bm&X-Amz-Signature=05ab1a0862b974545a3f6ff17384df7b0d198dff085a512599a71ca14d5c9c26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

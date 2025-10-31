---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TON7P7LQ%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T180115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJHMEUCIQDhNcG4J6Je%2FXJi5lcXTIhaK6uh%2Fxy25E6Dcx%2BGErJXdQIgDzIIlPw7VryKf%2FMm8wvjv31DMwceqDec2Y6UFqkkOFkq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDP6d4BIhGw3vXvIKTyrcA5JU8B8t8Mf7yUb1M2SPlPsGQv2kMRWiOZnzESHTsV8LpR6S%2FOgzWcoGuixbDF%2BCudkjwTmUu2ozBS3DmZdxZ%2FbrjOAfzj%2BgsYrTGOCUka9Ltc256T5fwndWAvclkuKHxAMjotcaNJuw4a5C2udrJkXJrGESOrwLpmKrloEQfB1WQHLOiSXn1JccsZVG5uGIpJjxB%2FW87Tw6dv3Ex0AOrOcIoUvmF%2BWrXRtegrfWO4RoHlrFCXadR8X3YUNuJ%2BsdGU8NvAS9kHmJyFB7Rb6JdMkX%2F3hhlf0NvrR1Qk%2BL5OvL5UgWnk7pZAQmsT7y4CHWAtoMTufyAJcQpvveZTtX2JNkW6E1ND4irTI2NuIZvO7zMh0omQI0qQSSODklnR183iTdwPe0Al8aO4n71M21tFJJckcGY6n4Nlf6RTggB4IHcKbaUi1o6MJFWRH65TwthoxZgF8fWvj3Vfd5ZdDoNRvC6U3p2487DLv%2FgXHQUw2D7vJTJ9HigNcymDd8l7Jqm8HkxiEQMMvPj8M1IYAXQDVz9GkodayarOIH4CY7rRYIAHrjBwSzoxa4okMSc%2FQAPTTq8N595ujCXO6%2FWsB98q7MAymyaUXzjc2eLR3FtKfuDTa18e24B%2Fnnqb6%2BMLbck8gGOqUBg0Xbze5gCfdbwHjfuLF9x3C4zxBM%2F%2B15gyMLp%2BLdP7Wzot3s84LITmI5piyGaokKLgy1dtv9I8yd5E6gGFO9AZhKt3C3PbTg6Hsyo41QtzGQ5aycdhlN2byrIdyZZyCKZhG5vHDjLhtb3mXH7QpX3g%2BJVEQBPp%2FClBzQdCh1%2FdJyD%2BpNWQAyuPmm3ETtL9h5cXQckS0JpdkNLtLDMp%2Fp7cNJTxrO&X-Amz-Signature=84ad0358b87639fe0e0a58bf662c08b74f0bead5e2b2739a2aee40e918d9e41c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

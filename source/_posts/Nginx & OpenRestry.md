---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665WR2PYPL%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T010045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQCaG6rhKalIeR5D05uXLzgwu1pb9xgXZYIEHYktzWpy8QIgDBW6UTwXL3xFc00b7b7xtyQrRxzPC%2F5vBrCv6v3IbDwq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDGu8SdlTL%2BX8DDeanSrcA8YRm6CRGr18BQgF%2FlkFgwxT3ImXCUSQ1bCgwyA7w1OzlM7Xcx5LA9EEprdee5yKxweju0BMnNKenvsWAx8wzVofm%2BmR2DP4H0NpTw0bJnqZNENz5z8U%2BrfCv%2BPRZsnAag2ZqG1xCa4Pw3BpE3ecEIajPzWpCvLI8M97DwlvDZgxmGm2%2F4%2Bb1n3qUfuMPEUjgg9K2e8jbP5WL%2FPquaT1A7ixjWBK0M1PF67b9DD4ugaXe6LzOpzHHIXO6NvY4LfnGFNXt146ahwB73F7%2FVByD5cHONmsihbTkBs%2BGgMOUmLqikW7MngxzgbcM%2BzGUvzoDvy9CBfK4i0BZkGtRzQfjwKK0KlV8Ag4ugaWJF%2Bs6H%2FkjTRsS5VyXY9tTxXmhwhXSI9JFrdht6cd7btqcFwLjFWjHRuCnfLe4M5vJjczjOsLg970N3jXiZw%2Ba6DZ3U1VqkaTIK3BrQM9TeDakoRXyCSqusn1mesqj1A1aqdOBcYZtGN%2FZKA%2Fs8NNn5V6KV2xgvq3W51cHfuQmA2uqRUTeZsaw9BOnhaf2gl91fisrEPKHP1t7rR%2BHYrEMj2yl8q%2F8aaFppMcxKWSXUivsvXlQRaMvLMEhw7VIwcmscabfhmIDZ3G7UxIbHjO3ULJMNXhlMgGOqUBc41NwUyCSVh6W%2BBc%2BnmbXYDtvxLWjxKXD6nukm1UgPwzdYGeuWcVlV0fSgqHR%2BbTUwo9HucTdMd7VvI6wr58ngXEHRnIYc29PdZcxCU8YDXIQ1D1Sus5hs46wRuFW6ptKpwmseiY3JNlpPPJcNLSIlwBOVaxO9DMFZ7Wf5hI6A7Zb8g0i6u6TslSjUSq%2Bg%2F4xK9vVqA0drrwUXfvGVrq%2FshQRbjD&X-Amz-Signature=c164e09856f1dda793de868d6ffa823e196af948d422c71e8b1e5e8da992a018&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

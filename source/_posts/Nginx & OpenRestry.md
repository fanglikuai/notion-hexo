---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UR3NWFRL%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDTpmFLCzB71TFraJH%2Fe8Sh%2BpY9FmeH71Tgqr5LPc1yHAiEA%2BAQI%2FgTCCPocPBVuAbDWqCBT5sGOoHsMlU3Y0I82JT4q%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDIvb62czUzuZNQ6WVyrcA30OUbv%2BzEW5gIw%2BITikcLUYLAOaj3CUFILcItALLYIdtzH4B1GiMBJkKvUIxsDj8iU73ncjwpQbG9b4xA%2BLhnc9r0FLDGaGD86i9Xs6o4T9CjSkFvraLVAWHxSCHwL9WObEr4JEP3rRz7o0%2BXOoL7zLxC7GH5c6VrL9P%2FQVtumvO1kOrT0OcgcSy0CJwBqyl%2B9mgMetGdGw5iBH3m3ZO16QLLVbFRVvVWJyUbdq57WvrRCUitXGLaJiIC97lzLlCSZqHzGly3mtw1frlfOVbyRRWT5irFqpdVA3gZtuOtFlfgRbeiwb1I1BzF7dKvICT7i17c7hxHb9H5QbjJu90VrVgkb9dpt6S%2Fw0unReZSabMB0DVVxCizyRFbNLPIw9OXjetk99eVj2Krp9Not%2FVyCUn4pGXHu1ZyuuW5mmzvM64eqcTJTTp%2F6aZlmBoFF3HtDkmS%2BvokOf4Tzs43NqK0AJ4d5UMoQflJmIkuqjE9j%2BVge7fmsWnLlRgoHw%2Ffnuj%2BWb6iB0hBcVtkCwdeqkBavxqr8uMkMrPs5g2MriM5kyAYqiSq0lXmDODftnPbhT9hjnpwxTXhEEsvwlwB63gBi5qf0CVGpZl2raOzjRTHq4RGvk9cNDlnCiIDlaMOqDu8cGOqUB6BbkekKcr0Juyuf23Vvs3h13BlVjf%2FzPzI6tmwMVN%2F9ivVpg8VSwWUfNVwh%2Ba1q02n1tK9SoMqpVuovVUR63OhDILHf2SBroo8Wl7SD0RBuATzpgGz1UwicQlkMXaveDpXlkqvRuo5%2FF7t4%2F2J%2FtS0uqUjYPVPp0LszfvtJ91fJqmhqy%2FX8qjuBwLLu%2BtbQu6GYOrQafD9Ug9zzqzqCRnRlbdR27&X-Amz-Signature=96c3522cbc215c183652b07efe96c95fd11e4abcb4ee489ed62d15f62ed8597d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

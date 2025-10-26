---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZSO6Y4NM%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T120056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCFLsDMkxZ%2Fp%2B0qi%2FtmIr%2F5jO3bgSc5H%2Bs6lzKLnpUB6gIhALovxr4yupw2b%2BI%2Fk03QNi8XMDklIbiii8MUS4mHUrC1KogECIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyko7PcjKHoOBh7l1Uq3ANoyk1iRlnrmVb5vyGfC0XvRidzTdT1hawJfC79TjxsImEDbDcNIUG0vr%2FucRq6lQvMu1TF4aRS2VElqCxrZ2yh2neKWHx81C6P%2B0eysoykCof6yJO8Ia23ph3BtXL2eVZLOA6VtLWCr4CYuGbr49tx77QyCcqsHdAfJ72DEggVHUhFTVvvPFR0jRCKiuE3ACLDbOrmNq9tl2NMEsdcF2%2FJ%2Fh3pSAEHinNUCu82RE1V4bwju%2F7TE0wyM9VIGgUdyBi3Y95TNejxySzM5JWEaw8STbd5ytuGCXGVi7ubREq1dQ2PJZ8YWL%2FM2tDFxXfdW4uu3VvEGa8oTZOAIeeddaEgbEPqJd0%2Bjz5nYyaRCIH3vhUx3yPxmhzFW%2BkgsKnXoqAJ8AwNB9eJmY1qxV6ZFPCiw%2BAX3eeampUxZQDrqnqhr%2BKGcRZN7FNmecBb3T3e7bljchqW4gTJJ2oI9szv5khz%2Br31s0ku7Aroc4N8VAyt6DSkYpeD7fsdbxcFkliw%2BPQeSvPHUN69GsUPgw7kJecC8pcBKRjbZKJ7lun0O5sgPxUqPjl2Fm1KfNhynNFL%2FuySyNGzl6pGe79wVK9fNdVuY99l%2BUgxB95gmOLBB6K5j6CgKtGMGy0x%2B%2BkfQDDQ%2F%2FbHBjqkATghST%2FxJlKTLdc0%2FzBAnz5%2FT6DaJAMHq09%2FBxIinQAeK9xsDXhWZnTccKzRriG5heSwBStuBciWoEqbmtUiX%2B8gqOs%2BYi1grfcQ0LMW5VUEW74tRXR8VjplsQm2mEZt47qgLxcRT4a8D80sNteDyfepgIQve43w6Q6MUl5g3Jtf8JcRu0eO5E4eOeiSpefmwbWy9UomGKtpEbqcfoFQg8%2FCY36b&X-Amz-Signature=a6a80f70b119823c12383dc384fbeffcbc917f42244d08173dcbe0dcaacf2140&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665HIIG4IZ%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T130054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCjPqbcfmdLa3nYF6N43%2BEkxhQJPHfyTPOY7pwfxSpHDwIhAJWCvuL6QFXT3bWz%2FEV%2BulT4xgg7JGYc%2BQ3P8Nh76a7BKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyu%2FaGd88b%2BgL8RoeMq3AOaHAHzR6jGwjZ1Sj1Xo2g0Sg3lIJb9pF1pRUCTTNdWyoC155HQ6ghhirrojqLMSuEblzl89MhYuKha7wQr65KJ%2BmEq1dOTa13HADadHBZ48j6kWn2ZSkBRXSQhF4P2HEFQyGLv3dNs5o4zO14uEIgzEGG6fEMWqb9fisfE47LN9QS1e85RBCTJwBMVEpBEfH4fh8mBqivYEw2%2BkDID9Zq%2FaclV%2Bx9AZ%2Fxln5UA%2BI9ZdqaQ7LLwx4oziHiqrgylK%2BkE5sPFV3kLEZ0vwYfIxDPJUSpXHzpLPM16lFjr2I8qdTDBmjAMD%2F%2FzgY3BwO3P2Sl6LS2jX%2FQyJyGC7KpmVM2rKt4oZe8YhdkH0cx%2BJ3FGmCXTJNTqnUnhbWT%2FzJWFg1O3eferPjFCFRz6LGzsNwAVnryR30LdIhswyHvH56ThGsguO3nO%2Bj81Wfae7iYGZ5xN1cduzz3Ox4D6nMU5eoxeX9QnEWmNxkE7OQzKhkec8siqd4b1Y1giNyIqRM%2FYwxUi%2FDNFVeqH%2FQZXyrdpnYJBvtZ0tOEk4O8J4InFjFYCIf28CVK3yuBONUpsDO27IqOFBzJZbuegUnffVyySG7G218cUhbqo5nMfre%2BAlawle2zg9gFbo5NGTsNDLTD17ZvJBjqkARwyfEHiN8SZLt4h%2BT%2Bzs9F0X%2Bmt5w2cP3fPkuA8TcRanDZrOw%2F1rdLiCIlmtwSAGxDEZuCYGe00c8%2F5mz5qQBt4efGEl2Nk7ldKLyj3bXGZf4MlEy0xWDhfrY858TBashk2sR1802GIpC1tmtTPxI1eSWwvOgxpbIhgsr5j0%2ByxG5pYoE7%2BG0%2BzdAskSFtwPrd%2BjNVCu8J2D%2Fbbx%2BICPUQeZyPB&X-Amz-Signature=0ca4c4af35f1b1d58c53f3044af3195be26253a177deacf4c4f6e57534d62287&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

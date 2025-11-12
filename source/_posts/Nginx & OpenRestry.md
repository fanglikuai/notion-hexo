---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TOHQYGWU%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJIMEYCIQCZN%2Btku3QtqSUJVty9PsgYj%2B5zbIQbBySX005LmIBkQgIhAOm78K7HPxI2OQXjqpEfH0tFjzDIHUgM3Ts43kWSPlchKv8DCDIQABoMNjM3NDIzMTgzODA1Igwn1CxNRbg33YKvRpYq3AMM%2FWCrbVlL53uGpQ3xG%2BdpNIBJDJ745vIHfiaxyxuCc3khg7EgGbgoYgVcaI%2FzR%2B2wuPUQrknzHSBQCdh8cyFW4oeLXxtFquAoFdZz9NLOjdahHyZ4JNIfqDtpmlX3e7CgtZCEh1Rh0jOK2B5kKOGtl3KxWjQJWnni4dLhsdr8T9Cjy5B6NEWj3rO3fCquNap4%2B%2BKV1AKL50uIX2IN2Lx9MXUNVz9VQ4S5s2GxyUiQ1XF4LCGerqvKVXHfgnztXDjoQ3%2Fpw2LLX9BjXjzCtizRjG3gqdLFfZOc19%2F8HaWTtkg3DmZP5%2BCnMaS9D9kRl9kpjH6OuM5Z335G%2Bt5bGS5XjZe8nD3rxrENr1sMMepa0DZE9ZmloRURG%2BHf%2BfnNRwLrZZxIsUa1%2FoxcVDtFu6rRRUTWTsaZwT4tIHED4sWabnU9aCs1J72HTgcF%2FME81UBy00fii0xUxc%2FElXu1VbeXV2KdstN4aqCrLfs5kachxwYKodBgZGXK%2F6qrMPxw3MGDKbUwDL2jEZj4CDsaS5CrvJnCxUsKgOzZb%2BkPwbUKfeLzk0cs9wXzsK0B3dNxQy7EaY8Nt%2FWg0kpy61LJafnRzms1dPTN4nVkJNcoJt1MdkU3IXF1t%2BgvKycKpDCrkNHIBjqkAZTeK38YvbwWIPKWWjc9VPMVNTWP38j6PAc9Uo23ETVWwPElA9zPXcktjvROmA1zd7VG%2FVKBGxfqcV4kRngI3s55KTk9wnxwnUjqOW1NxlI%2F6Zad6FACj3HPQnF%2B7Sesj8eoqIryMiTLZWt%2Ftess2zozPZ9mTnyop21jZkuS3Z%2Bb0EsU1n8H4UwESPSf%2B6Q4IWtjbRRdJ%2F2XePkg37JbQjp%2FSWMd&X-Amz-Signature=c964006a8d70892ba2a26f228b7c7637b179e191b73e98615e41ba88a1cd9047&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XS3LAL74%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAW6AkXeOtSQvvWfBeE189pJzfx%2FIa%2Bmy5oHmtztU1C2AiEA6OvOi1kLRFdT6BP3QmS%2BT4ih4CH2lka%2F4JhC8qNBtQoqiAQI%2BP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHesuP9ZdIz43nhpnyrcAyYVLMTtjzzx1HIKn%2BgmRl%2F8dInxpwgGllrKh%2FZIjuOZqr%2BHgOLFB4HayIQbuVs8AG8Ad3LLOo7dJ4Ip0GJNTz99HGH%2BGMnUvI7bvB8R7Lj0fr1xuijbU7p0iL8s8k6CNckgvpbfxIBhbUMV%2BMpRgRSlyxMDGRfChpH2j6Znl0YjTeTCAi%2FEh4264nJN6Iw9hAFmUBNRcCrKyrTK1m8PpGP7coMpCamD8clvqOT2ruwZ2V6mNjY5cVdngc%2Bpmta%2BPUKh3%2BZ%2FJf2x1Bb3qhN2N%2FQr%2BhfYnk9HZ1T0ex0WaHAs5Ofnx%2F2ri6unTiapDj8vQyPVQ0b5d3lPsJkXb043RawnX%2BhhCuclKDS8%2Bxgbj9Skh6C2POP57iTSjx6mSE59W248evSYID%2FrRZPsoIkLhtv7P28HrBd%2F9%2F3RkcPLafVt7wQ2ErOSzaTE0JCEuvIUl7B5viRcwUYQ4n31goIWx%2BR4mZeiwuVEiI98ceEC8eRU9AQIO8yEhhStGud%2FMt9WohYdFr%2FRByQjlysZlnY5xBCdyl511SyYVV75oDNTJpDS8pnvy9jJnLxjXtuC8VNEhB60UVV0UBa%2B31K2GmQ0cn0t9yIg5dYCn6R40FqlYtS4zf1A886OiGGmAid7MNPPj8gGOqUBqgAs36rkZfsss%2BUk7QoEmURCfipYsIS9sLHNO0p2IQg5hpFwz3raJyClfjhkHzOy1wYKms8ep6zz%2Bt1bepA9468ERtP554%2Bww5svI8gE6NBRgUso%2BLLEZnpPekCzdkSrxoj9CqWuHtGlZU3r7YUUaJdOeBGxzfYFVGmrxclOmHZf3D3FT64vdYTsccWRiezSk4D%2FJ8apScudRjywY7k%2FvSVlzRYG&X-Amz-Signature=010a183e9bc06335251c47d208df0c08759fc1efebe3be0db1b4e425cceb0a51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664YFVVJMG%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T220041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJIMEYCIQC5fY0Poujfea4DpuByQreTwjE8osayFOB%2BrFWVzv6N2QIhAI1tQjniAQowgqJHllflDS%2FZg9p3k49zJGAb47tp9WRNKogECPb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxT2QeEfz16Z6BLtRsq3AOsoDmy%2F%2F%2B4vQmAf31QwBRh8iEMjiZ7wBTjyaXrT1fe7t9jSWJntWs%2FsGFeFY9vEUTjB2XhT8pkqs0N4VMSbhd2c4CP%2FoZmYfwXhrVtglP%2BlLNuMdd0wKtucCGKDz0m%2FA9BG%2BcO5i4HEuFSEmELByPwWRLxAQzNzHbD2r71uRYyK8NiHOlkgOgksOa%2FXjAiqelKMEiEFnus10O6aAzHWx5ZITNMck185dJugLW0W2ZQvE5W6zvMjlgrPAk1FpK5Lrc9wAapESbanweE61h9KWAc8MULaOxQ6TXElMmeThTy0sTSmU050Rpze8SfEqDZ%2BvG%2Ba8PzhAd5FrFkeQUrEPXp65r8jnMXi8By2U2939yRZd3UNimUaVFhtrGhI0Ax1tkdh3s5mKXBjhzbuIvZC7dCCJWtRVFjjQFjz8X%2BS2oORSV%2F7%2FAXEN%2FM5pQ0I1fjX4BVR4vRNKqvwgok0wlwv62Ctvw%2B5s5yjpFZ%2B26CcIWS5OeRrqwb%2F7UZ%2FJAPFqV0ePd371yT3eHkavRBge1tF9CskAneJ9dWc%2F5BjPL%2FG6SffhEB%2Fi84Su052laqM2jz10q%2BcKInNG6YpsEDDd4aY0AD9rZhvZrRPat76KAGhRi%2FpC8w388BFCRwbRPTtTDIsbzGBjqkAX9JBfRtm8PrZ654tq9iM9ZIDx%2FSP3mBEQNfWz0zHhv5K5knKyjAn8Me9qJOOYiYPNlkwVoq87TvTomjmvHlGvD%2F42gzAaDVmICBpbKRqeyWZjr8O4ZcI5kLpkcNtLu2WPq8bgqZMBqgStf5c0vyvfk6%2F3XDNUdcuHPZH%2FmVLxr3HVLTcLeH4nhiOU3gAMNGQQN3n0l3YIBbdgUynqCqVeFWF2w4&X-Amz-Signature=b105f75615f6c86e4be4629fc357818dac4ca4af13d61fca901c53fe6189aecd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

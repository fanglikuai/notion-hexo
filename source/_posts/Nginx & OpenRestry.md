---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SM7O76AP%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIQDUx6AZKS9z2xAnX%2Fx0AoHLYUAXvkNWhcL%2BKl1SYpwfqQIgXW9cemByUR%2BgwdZM%2Fw71bxGvImW%2BlWi0iNJg6OLcGmAqiAQIs%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJa5VLpKuFLvp7rWPCrcA0kY8aK6Fg3TXJpmt8GX1C0%2Fuu5kdRvm%2BUB5UZXARijQVsNyRVxsKokY01kVsJ9KSrRMUXTDaWaXN3JCGvc%2Bs%2BpyZfPpjPS63t1xo8uOxjj4nRVIuU6M55xUMo4Fh5VFLA8kJ2bWHX5fMyjXbV2oGfvS52pJEMf9klKANVsUAUO65TjnE6nuElOCTRznzqB3F4NAlyEPerEDOVbRqPZmxl%2BX7LqScrJNsLk3c9VJohaKnx9JSKW1XfYHU%2F804%2FZR8cHF98Dfr50kfCwKW3jv1nUimLVy%2F3UU5UYhggVH2qYg7uatIbUegMdgok9Riq%2B%2B6VpZ252aAhtYxdAiYOJuhEejXTEdMbcovnW%2F0IV5Odqm0345UqY0lU23iUfmDcOpTseH8Tnr%2BWCk17Ibab6k9t8HGuxC3HP9wpKkdci%2BTAdrVk1dSus0vt%2BHnvP5P7BobN58nHy3z0EgBE0dv8niJQ3Cg1DbVirtFhzeHftdp3IKtJ7upp5lphqvdgAeXD3B5x2dm1nzHxeN8SHX%2FITXu4%2FA9uAvHQ9XYxJLylKsqSQPhF5vQv3D534gOFInfDpmwN4hzT1yFx9L00FVE7VTh%2BJZIqnK5qJWXo%2FzB6deNjSp7%2FniSO1qYO3xbo9OMIfhy8cGOqUBpbWBABgey0OuQLVj3LixrC1LHbzF4oKpe8oAmV7Wbcn1Ct2n%2BmXAjNcPCOfnDNlZOPUjHf7UQnsfqui%2FC2Y%2BT3Y9SvLeja376n0KBwsXvKi0%2BykwcxI%2BSMBRuc43aevl8XQqxtvcqWSjQ2wZ9Mh8wPTeVLasqk3YWkYG%2FnMoHWAEgl6Ue1gSfw%2BKHiRpneowEPB%2B88ehUNdr09f4dtaDeEJDF7wA&X-Amz-Signature=eece0ecd504f02895b5293622eac90cf035b99f6a50f64c5ce1ec4b348e3163a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

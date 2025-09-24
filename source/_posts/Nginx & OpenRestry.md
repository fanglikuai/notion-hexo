---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664C25V23L%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T140120Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDLySIwcxum8w056%2Fq55OMV%2FwbDkxawzAhcG0JA7hIakQIgROPVBeREInQ5UWTSs40PEJR71vpGMpNgw3DNfBabb7cq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDEW0iUTDlN6ijlhxCyrcAzykKFvNKl2zCRMYRwhxsNFHkBtzR028lhZS%2FZMS7oyczOERuxq5A1IV7Zg%2B%2F7Rpvc688SIYrFQNqA93tRqOj0D%2BS3IMfdGE0iOExzTTClGXAS5INUdqiSq9Lj%2BslvL5koFa%2BTbtc7zVVKXS5YAX3gzZVZyKMBY2NoZd7Dm9YKFv%2BYaVqmJzDE5k3BcOs2buDKpyhjqEZcrmcQoWGUNP0HrIqafOERUU33pxilDWNwZwyDxx8oKn3WMRYp0l1dCyID8wVvxyl24v7m9kS7%2FCD7Yd8b44QgXM5eyBo5a0mYbe7GdVC2UbdMgDa6l7Qtz2pi72ngVoc%2B0XljOz9fN54TjpR90mJZgahBgBYAbLH%2BeqK2nM3HIi4xiLx%2FaqFTrWz6Dn3J3CfJdyVyVyOVsCOXtzXso9xt5J0orvlkq5j5K8vXJvSV4j%2BvkA32h%2BLmvVGUihyu9kPzbPogSFaMSHwcxNwW59%2ByVxekpzMx31SzQNJcRqQSCr8SlZkQ2gfugnCIqkEazYz1ea68nJw3%2FLYiI%2B2GD6kewyxeqgS6nCqRhOj6v%2BzJ1l2B59c1JNlNZ5JHxMzE9RocPCNouczS%2BguVfei9qDMYJ%2BlbZLCzdLRtdnTQHHmYL%2Fi7YkYviPMJvSz8YGOqUBRgWmLzNbxcaWh%2Fu1obMhez6hVZrmxIc%2Bd0EFyZbuAr0umTCCDjBaqwTwfd%2ByEvuX8tACNRLJN1E60PH6uOngSfDCH3C2biIdyyWGtGwnZUPPPlUX37XdUZNqYXvPFSKTAJdLEwaW6wOd0%2FzPzSw674SOe%2FNTM3bYkLMAyYBm1c46EaiZksHTPVZQKJ4OcEVu1B9TewfjW1Yc4Tr7LFn30RFXL5ot&X-Amz-Signature=f6485757700f68877bf77e01d3869c61e990a55abbdf933ff050051389b941ad&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

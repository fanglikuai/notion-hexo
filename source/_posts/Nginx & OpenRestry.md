---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UIV2EQK7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFZ2vLUgFCDpncRsV13SuVnl1FkyEg4vFXYJ2NDBLWHRAiEAoAh9xKcpiMR9EzZdFhWxV20YYBQ0IzgbXSSmKx1rqOgqiAQIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL%2BlmowX2Z7kSjMaTCrcA5xS3KhzX%2FT1mzMZ4OtjPQwrJg2w2V7HM4%2FegGZ4b3AysC2kiXvel7T2etgSdvBnRIZrKJpc%2Bdky%2B%2FersRtN7r6gYJDjmQ00MnPePDB6%2BYvRqoiKRHpuYdTx67HjCy8JLrDNHX%2B7MADF79yfkSTCKDC1AtFgofUrZEMH6Q%2BxavBIym0CAwMqIIIL26FZuWbROnrwTc%2BmFUzfXga4TaCxYAIrNismZwPY1t3vDE7H5dBM7PeZtVMEq0WHPv7dhKN85vUGSc1Z%2B7e%2BIJ1we%2BK0dIbMd4ICcsBsY2sdIgoUL%2BkcjhJc7Wl%2FZ9TWjrwvipaxZFP%2F3N3F%2Bof8tmG7AdMqYUxmVWfvFdAerdC%2BpzXjnlnVR85QFVLccrRkB%2B08H6ACecddZ%2F%2B1u6peEgZBgEt5fPNi9Uul6viKrMlFXWdd66tCJ8PDysskrRjLdsTgwjt4%2F5OHMVB4CkP%2FOqyod1oMEZbH1%2BsYJXFeHbMSa%2By1zlxlrEEHL%2FGIaAmpGodyuZP01HjM%2F%2FELBsqbd%2BQK4wiRH3doH1hrUL8wjs655Zc7WZBeKz79viZsYIXBBfufdP7UKyIaUQC29kmUBkfASmDTt22xlgDp%2FCastZrvTZ8EqVONROlU0HC50Vk4rXPrMPb0kMcGOqUBP2tc1QtTTrYGUSgNdlZ2guif1JxreTBlAgLCDqjnOzAOkBlVeyqVC5e8a%2B9AcOWxlJ4%2BykyXNLlLcvF63ZA2soNV16QNO2z%2BpfIZ6ouZnDSlLSn9Dtiks4kTEScXB70r2DTN7eq346vxw%2BhtTrCdp4KtgrvfUmfuQ4E1NWHlVG2nO06tEPpGsuMrGzy90bpfwkn5dglfGPyVilH57aNSXYbWlreQ&X-Amz-Signature=18831c10916e6ba27083a66736e300562761082d2f665f09963c9be06ff70d9c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V26M7WT2%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHJ483PcWB%2Fid36DtKMxMe9KDyYxyMDl4xW%2FSb2kEx6AAiEA5daK78KpSHg7I0UfCIN8TJaeGCPDRYZJtQj1%2BGGeHvoq%2FwMIMxAAGgw2Mzc0MjMxODM4MDUiDMr3V%2F5Rnu7luFOvPircA71M%2FYmDS1cpybDR8LWhrITeorjSE%2FR8w2wgbKS974mp5Exl8Q4fhP6dpr9QIsxPUqF5CWrqQ2v2kuT%2B2ZGFi1fSKT2MshzvJFFTabtp1vl6Y3FOZ9xii3sbWaU5IO%2FPL2I72t7WshW35vIIML5TCrOP1BgEkDejNaM9w3T6V7nmSaK9OPfitSzHOnUL%2Bf8tineuTuCfEruyiNHGUlKihIiQhKvsZX8EJdmWENyY1Bks2gav0XuHSlHJPM8MZJZZ9iAd8U0BBKuLm4525NX4OOzN4IAj4khJyEZfHQPMfV8u5XRhIiy2xx42bOOV5NWESBsq2twTTCBTvo4s8uNwlFW2SSUKCpk%2BVal5l2p9wJ1TbGKTvEneddSAIQuOm%2Fnqn3R7PSpU3mwFL41b%2B149dfcwWsNlVs0ygp4wrLXVD8D2rMLaVYRN6wtbdIZ7s7hPAhtHjcYfe8gKlQZaurIbygw%2FxaVJLqLpNO87vp1DyOF5TDO%2FQUKiItcFdQiE6OHYBblto6mNP2IcSllPPsFPl1zrb1X%2FGo0TURbRHE5ZnVQCIkgjgTbslky2h3cm2xni6IdywMpyyprkZbRMYsteYtlzKipcG4CXvLWvMW3HemABiupNU00Sz7DtdPv5MMygxsYGOqUB1DEwi%2FX4hZd4ObazRvKezYfmTN0ixM22ZrLYvr%2F5OhjwwZkfdyNJjID0N%2BOzbFFOyODlmuFFCxMKFyZxhDv6eFZ7a4Nw3K4RgGGeIIfY%2Bjhl7HIFIt5ny3Op6%2B%2BuT%2Fr84DAdBDHXNntIm9rSREwXwXlv1Kt7yCwZXLgtGCvBAa%2FZzNGXmVJZ5gb041mBcUNeDdIr1iaS3a8BsLf5WjKYA65WlnMV&X-Amz-Signature=d161b74f8e7bbbe9d5b5a71283e066b5661685ec9d505e42282a7784ca71a285&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

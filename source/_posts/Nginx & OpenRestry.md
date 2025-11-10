---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YHW57GDP%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T210040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIDPqijqzeC%2FrBQj8JqqgoIVVBS7j%2FVQJyixBGWmRM90gAiBepO46pROF9RStlvn35dnempjyM4gexXFxvZRVDk3MsCr%2FAwgNEAAaDDYzNzQyMzE4MzgwNSIM1%2BiWHw5mYv4bLxmQKtwD66BFEfysSY12Xby2tuzDDva2pm%2B6BqLusabv05TMs330%2BkwV0FIW5yDsQaR%2BDNcO2lsIk24ilbgNDbON8%2FoQ%2FcDEVFLIMpgbotPAfbSQ3aQMHhvySQ9ksDvg6XgMJw7EnX0AvHxZhLlaIMehMk9lnbzxAI5%2Fc94zkd4tTIvBlgtjm%2BGjq1PLCmjnx80REreTc%2FbS%2BrQm0nHT8Tkojt%2FUfuLqdv4A4CmsyWsUAdk5qRcX8prL1mS58hyMufHh8YFSqeh5ntORSI3MnjvQh6XzaA6cGE3SHhVW98VuDy1pojvd2AdN9UbsqLP2Y7Da8jfpoPFJcCA8w1%2FXuhHe6lX4l9yMEXJFq8R9jictdmDNumsgzQtzMM1Xuxbo3uiCpiIoSa3N1nvUNfbbGCg7UJExkWXbBrB%2FDR%2BA0AEECUifzlriCZ7VjdGFwFdlCnA6raKPtssXP6tOfEVa8lr3igrhfST5pXTxLum1a5c5ztr4rouj96lMbCMtWYBOte5wBClqjpE5vBRIrGTDvic5xGoeHCKrENLQ23n3nptLvT6CHU8FqP3UYhfdkq2HF65MU9VPe0BE4cEDd731TGmiRtPbd0%2FHNhS8MxtrE3zy83zGn6bdf82Ow5024%2BeyUaswk4vJyAY6pgGAjiLDAd%2B%2F9fyz2ki0KZId5ssUwfuWcp97LQ2Bi7GtlU6l7OsW%2F9qv2BoQ2vTyLYIAX3E9glX3FHXOYmnogBDPM0YpnBOcCH%2BoecUJjSlVGbqBPjcPmE9bcXi5BYaA1zQpkC7fg%2FwNBJ9OcRUQ4TCid12BVqOsVMPQTT9Y75pyiRQ%2FvOdqqges8MocInmw2N7BYvwfCBf5pxhTp6A3FMctiLVNjb8E&X-Amz-Signature=7ad5d3a54c4146477cc1d1f91e7bf81690009d23b22f3d158c693c6397190354&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

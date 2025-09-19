---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672Y5W36U%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF4aCXVzLXdlc3QtMiJGMEQCIDvimbkKbfjwyrBPQ2U%2Bfi1FAX0cEq%2FY1Usj4Wd0TCMOAiAQq86VmGf6OvOH6DiFoczaQalP4bpm6GpcNRWwn8VTOCqIBAjX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BNTIoitjIucLb2P6KtwDFceBUktuyVs7lA2SnGWN%2FG2bPvovXj8vQFjv%2F8YPSnPcAacMRj1FUTNMrpZic7t3CaJIg3HCqCLsAmp%2BcaFTMV0ICGJtnah9jSZQQWcJlSMn1TMpqxNPd4S%2BcYqb810%2BOc2cIxgHzQ3fi1%2BRSIFri%2F25Gv4lpYLykAqmBjNWw28uW0kiIZA5EpZbDSRZDa70lsi0FFjda%2FCyDyaOxFs24Y%2BND9u73TAZaFTjPnhnmo%2Fvfam7q0awzh%2Brqig68Lq1HAOVFDIvseWG0xWRM80btbdxCiC879AyxCQNzHfVXFA11%2Fm69bpmIjip%2FS9xs13tR4XrCRHMngI87ykuo%2FrntgsH7bRxPPfCoufOMqYB8mza0mXpYJ80rYIDVubUrwjDEpUiDPoUxmk%2FKluwmcyZMGxS1WQt3LF38p%2FnS6Z6QlA250gIgdnAmTuTdBBpDU4h9Di%2FdMi2CknoujLzQ3bp%2B7gxSL0bUbRsKuC26TCmtyfsS5u6UyeZ%2BK6CFH4Ux3yatp6gqPmrJiR%2FrOIx%2FJ9ErRfsfUnQ3AQEdV0qQOZ%2FSR5OJ4Jyf2BUqmHf9g07q4DS1ZFmz1%2BmN%2FOv3Lr5kK1lkrCkIVxvNIDO1zWVoC7FJUHSF%2FZu5hbtNL9vJd8wmsa1xgY6pgGPYjkTi4TR7VwH8NN1%2BOWFogQaL5%2FcVHbU4xHFzvyjUZszSrV4mrrkqJsb%2FJk8lAE0%2Fc%2FQZNAsKljIXEeX7awHLgDpcoB2z6pkeKHECexzpAOiLIs%2FD%2BWKJq9jv2n8X78%2FmuOqc4nhRpLkxD9zMe17NFOyk6%2F2r3HRfupN%2FGFMe5xKO2dqkigef6qV4Nbb5QiyYPv6pdZuidOeb9PudDiRWepQGS%2FQ&X-Amz-Signature=68923253268d485d48c4022101e704c6b8d957079160d25afd7bac9191f2d155&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

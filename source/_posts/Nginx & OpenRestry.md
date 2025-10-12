---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKUPMCSU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T220042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEz5ie2aGtOPXOlouC6GWYVNFv0pILcWlPNbhqgV7p8GAiEA0OUja%2Fz7xG7fwz%2FM8zUS35V2SYLUJL3jR18MMfzC3Ycq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDEvMLkbItH8dQLlVgyrcA%2Bil6M%2FzA1jV%2FFRpd35GqtAhsY%2Ba3%2FmeUgwQ4AxXNupdJKaAfg3a2a6hLRVrjooHceq2toRJdLpDM9DzwKGdqoYUyQgM2UXibahchM3gb6ERKLqU1vCFFRm7qiHQ6NWodZKbQ%2B2Rn3PjaoQWuxtIgsj0bFgbld1nJgSFysgyPVXd0ZI%2B2NJkTbcvhmotLOC3LfQrCUm20Ru2S%2BP9fEiLumxmmAxO1kqvUGCiBlF8%2FTo0QmEyh1MlVwpj8IV8zGDZ7IapvAHpZKbQmbIUNRIo4yDnh6NC8BoC9JxCSJ%2BIs%2FCJK0L0eLW0bS76TDmDVXXSUnPMvmHTNzP7h6wb3PRFj5lwnHs344GRToORHVM%2BxEpodw%2BU5O5AmiO%2BdkIrF3pPtNt%2B4t9eH2nM%2B%2FKlQStx1gZ18AnVNPaZDXimEI8HNSEqSUedcHqaRizeeuT8cBhX69Zhcz30PWWaOReoAiivRC3vK%2B7ig%2FWAmTgiiFHVPLDkM%2BodCZ3iZfjK7HLFNjyN1B4ln0gHQsHJZ9xxvgOOHSlK30%2FiL0n1D3cwEc0J7GW20MYxpeM0kNLifaNEL%2FqPrzVLi%2BQa4g3%2FxDdabEfKPEpfbJRsUjianLnl%2BwBHtsj81ai4j1vDE38izIV2MMOKsMcGOqUBch9ykB0OEPBwbpQS3h8rzD4a0YFQNGnCWSIFX7strqh8E%2FMq0tkpZTue30ARmnqgwoJhh0Q4uK%2BpFQGLyJsJeTa5%2Fu0VJu6cEaJIZJnj0nyGRcqas2Xo0PoMxDMKLsKeJli%2FpCcCTLghjcAVP4z3Z8dLky3lIxpnRlIDWTLLvXFzeEJfvIIuAQD3a3I02EZllAWpgT%2BDN%2FtpY911GyBFhg0OxLf5&X-Amz-Signature=d244bb078016a50519df0e95b0d7382d3d1c96d7d3f5df4e1679b983c94ff740&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

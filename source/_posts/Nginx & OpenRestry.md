---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664WK67PVG%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T200054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHNuvQgpEnQEhO9oUj8fAbffVofVJElcCBjt4YWNfugrAiAX2zobOJj5BBNq8IYByWBVyY1hFJLTPvNospcQScYc7yr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIMuybhBTeRftw1V3pdKtwDprN%2FxWErAzvQKkOu0LUadl0xmDuryS5N%2F8ESc2EHGhH45ZqW96Y%2FrYWF3ToxISmH0O5QyO7el1U9t%2BTO0mDTVfbUp1ISZZXq9iSeSh4tEDVkNmSqP2MlixpCRwvpWMDuezDszeJnk0lFwh1tu%2BHUzWAIdlHWSImH3nDxcpWdqdT%2Ful%2FpPc8DgS%2BgHtzvGSXZDcMTAdjiY%2BMcRBH6CKYDkN5BNZR58fRx0gue90F1zZDun3fjysS8wqQ9hSpcJwwnYDrIX9tEmcdnDBZOnVFD%2BKdA3MVtN2IsIH4%2F2g0L9Y%2BIhxuCiELZ6IRWRy1INQmSvWYcKW8AFytVDrN4XlR7TS9Da5vVXVcJrWeUGIGlpT%2FJoSCi%2FrvchZxVzrBMKFGx18m%2FaWwww%2BIpTi3Php6qTV%2BWMZXSgh559cAnZnN%2FN7y%2F%2BxLkZUe31gZuwKivNXOB09gNT10k0kF2aA6EeF2vJqTSrGkvVV0NsFIEAIDTX9FL1rpqFYPBCcrVzyrjngthPqD8n%2F3zn3Dc3ot786AX3ZMbkCpbFW%2BJphmG5bP6SSLm4x16Z82t%2FZGnK8wnORHzBRCVa%2FUr76KKL4SRo6dlcptBSyN20aJRV5vh8G%2FMdI4R1K6owWJhIOlhiAsw7P3QxgY6pgFm7LtjPeByGMm9S2rBg4vok6IoA5uiuDL8Iw%2F%2FEsQDH4qzGSfxO7pLP9tG5GRDP07ATxjGbF%2BkOVeNgfyvLB8IS%2Fb9yjB%2FNDDcPWz7CLrqJ%2BDIwmT9NZozrAhYe6KstoF%2FTsQ89IhDjXXqbu9l%2FS8gfMn0L%2FqmpZ7Oro4s%2FjyBi01qgZN6AosBFe%2B8fjnO8iaTLTHkpebhNPQ2zCGKwt24uCnr%2BeCi&X-Amz-Signature=efc9807b37fedaa9a2c9942317ecdc44285706da9bfa2b2c1bcc219bea368859&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

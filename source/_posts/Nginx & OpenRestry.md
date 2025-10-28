---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TG52J7ZL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNeaaatuQkQeHVCj7FGwMHxh808kJAm5okXwvs9Dwh3AiEA3MIPiyGik8PhxdEfYFrTvj%2Bm1SfkNetXGC6Nuat1QUMqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDLg9YWpg3uoVp1cYCrcAxKAE1SZWLIxmU4SdBZIZAFXu%2BnGMKqQDrRBQcLqcGs5Rr0UL0ncSYC2IjWvTjPjJvPgM4dA0lF2lA%2Bk7z8r41TtkSrPe6d4QWjTsp80ShQG6JIiiTWpKOxG5KoRziZA7PWxteXg12otwOgEzNNVe5ndH0phLtvRdLGqmhXnEsCXFlrMfny3kIQcreg9RGL%2Bvu6eAXiJVMRtPObPjEtH2vkrm%2FZW3ZliMK7XmosDNKCyRTk1%2F6pFNijaIKFBlV8ZHwEHjl4Z2EbDTwMEjgoUEpRz6%2FV98IeYHefrmRoZd4eeHVGHrFp%2F4wLu1SEe28JxVftbOBoYfcdzU%2BLvoj8264nllWdMIPMJHfwUwZHECk7Kg6VuXidBFUWSD9nPMGDKpb4HWjUYN9mCaxvT0w2p0b%2BKCidX6ywKU2amoKzmBXBZYmWYcOWutd1sCmFkH%2FdUM03VAkYq6U4ljcEMJLI%2BaeO8y6uoZLam6BRGvlbLtjud4FxrgA30nzsFpmq6I8umzlO3hAZKsh0hGpqB5pHGcC8WAIQhU%2F7o6ZPbHMUPtesmt6o%2FOvwhM9lZHwvY0gDxwH4bX48BIbnQbYcBjbobhmlHdWHiRrck6BMnnayoe%2BMhE8A1i3pkrRdCrsXQMPD7%2F8cGOqUB9bKF937J9f12RKo2Ak08Rs63e0rrkF7VR0FlF45fr9Hl%2BiecIYNpRT45Qhz0ZSQnvt9nBSxzaU92k5kPtLPxzLASEy%2FkNq2eNimqXl2aDcqwdjpEnacLN9EyIgum%2FjiZMqrpVJXQyCyInfjozfZEu76AtY1Pa4Z81whnRqmZlUYPFxxzDI2rblzIO72%2B%2BfGUqkDklFd9FRzFMTPFPjNo3JIPn0zU&X-Amz-Signature=ca726f42a7317b0f6be848b9efcf613785a8ea8a60343dea40abca2249263e01&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

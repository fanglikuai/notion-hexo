---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZCWYNWCQ%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1c%2BF70vQRD%2FUv2zDNTM8%2F3NdmP4ir3IPeA4%2BHsue8tAiEA5FAf0W0%2BtV64Q0ed%2FXiaRONC7Vk%2Bg%2B1WYNXhD9u%2F4sQq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDHk312%2BfFJRUkXYlgircA6jAGmEEUhgSKCnkfxGSj2XGx5a0LMRD82qlrNDhh2sh0HJGqDh4rXlvLFkseRCITp3VlBZeor51nyyIChMamYuXq69D8Lb0vYT5qg6SC05R0%2FkpVhBdAi3fiXiks3npz%2B0uFbAlMP246nNEVefn0j%2FbA5th6F1RT09fdo6Xx08s2Igw4WlodjLMA2zCkcvS%2FKMmyiO5Nn4o8J7E2AGGJStAIGWAPBUFZUhafCxSrbJOomiwzo69Jxip7c6ZpbtZ3UQ62%2F5zBvUYd2FrYNloiiLHvAoVxM%2FIS36cRvt9kDoQn4l3ULCnygUPKkMiIK5KXSmHYgjdjoJwl9pG2ZvDGR0SspaHPD1cFxBTEu9Pmosy2HjtdeRS3WM6Ulgo5L16dGAKvczsOKI%2BrdzKVa1bxhezY66tNRiuvqhPxQ7ZIKWof%2BD9CMvcIvYgVNFtnRn4Fl4Gq2GpDu%2BwXOcbP8R16A3eZRTxzPkGuNhG8nJ%2FOkx0Zx6hF0oAzFoAwNQAWc78xPAC3%2FieU7FL2%2BuXg7a7%2BXV1KH32r4NAuy0s87imEbFF98eaOZ4665JDJpy0DLwijZZXVwJiye0h2BdZiVFplN9cdzlvgqfnOGVGyFXAcPFBFEL2NhLQ325hqyQbMICQwMcGOqUBJBrEG32whBV5xPCNjlEaHJHsFLXCRN1SctfmDgJmcSv%2BnT2ZH39xAvgOHH96RChgoOI8pPbLtbDXQKhViDKvegZV9QqtTgHXX%2Frvgje40%2BUdDylhXyLOgXxOtppcsyHTAWgRw0e7DuTjBPts4zG4855ebyuNt09Us7irIxM%2B1OfTcVkryH2XxESPEma2LHHPEyx3ui5v%2F0bESgnQL%2Fxh%2FjFjLUGz&X-Amz-Signature=04768e1f12b887e5eb357aac2b0963f861bc10f6b5b5accbca0a7ffd832986ce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

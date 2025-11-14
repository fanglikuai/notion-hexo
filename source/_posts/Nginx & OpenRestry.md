---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662JOKITTF%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T000047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHSJMGCcfvO%2FvS6wOmGza%2BZX2dQwD7pHTTraCho2WBLtAiEAxstcx6s8LHRqF5oITWrneLkxvV4iZKeLDJh%2BRM61WGkq%2FwMIWRAAGgw2Mzc0MjMxODM4MDUiDBZppnuKVwwuFzttMSrcA3rOaJqks7MTKFuiZHgUP3e3iURI%2Bl1RZ14z2YSYNnRSESOph4aJZ1vrVKKyoiDgS4A2UROsldW9F5bqJAcJLEfZ7%2FDsqIXLvO%2FZgHIv%2F8mEtZ9yF1lt9yK64oMlHKW18t6lQbmXJRuA%2FV09UdHWKOQDmBH%2F7mQEDSDAd8ISuJrF48XzAZ5xjjgx4v62exqV45t3IrBj28ZOFKPbvCTq%2BF4rtclWPLPlzgMQdwA04k666u7l4IQw8zWQeVKBg37ry3Jr1UWRdLkvPPLbsRBFenzQpkacIxsCuHjt5D9T8IB47veTepfoy%2BVfmVD4tlN8SOBVo8zG4Dc1Mvyy99nEpLdLonT7TbveV1H%2FtRxoBARkA8Gsdxn8z1lajzUS%2FDYN2bPDy5RXstpuoeAvMPfQzM0XTAn1BlcsG%2BnNGZKlSD8FNN8eMjVZn9KN3mgKhU%2FdrYB5KRYusDzw5wdlNgmtGS2mIAUxLK1%2FlJocl014ArTO1rG8LyXvZQ0GowS1V7a%2F7%2BJDdlt8SWfgZeKDCHNynKkQgmXXg8aylJwXDp5UEKamlxakPbdjh9f2MaSOhEH9xvqp%2Btd0mbZLzCM1eKy8OaGRtE9x4U%2BF93gFffoiWJK1oGU9a%2FH%2Fh%2Fvcr1CsML3R2cgGOqUBTUR72DN0dx4PfgELwMeVh%2FjQ%2BhjcnSE61BeU3Fvhn9O01HZ12nKD7Dg1E5TDv0Xf3Z1gVYajsbOWvvWcUaWUn7yWco57UsCOy%2BAA95zXK%2FpoAq4qPqGW7ibEOwYS1%2FP%2Fl9Y%2B8lomI2aU5PhbClV%2B3d%2FC99fj56lSDuEU6XGyjGWDUH1F9cT2xTcyLHsdA0HrwN4QeNhPRAecTqNAQATAp%2FYdEcV0&X-Amz-Signature=7949493976a82901f6860bc57dd20de621816718cd02df80b5d2cbbfbb5e6ff7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

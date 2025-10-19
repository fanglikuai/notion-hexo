---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLFUCF7D%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJHMEUCIArjIJBXgNl6IsUmBHkLaEuZJquVuhvF9uIWiYkldi5jAiEAvn4Ecpbg%2F99YWLruOOHr0%2BCeHhpMdzksRpA4U47ALWIqiAQI2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFWn2e%2FDk6UIwZXhWircA%2FjB4bH3GgzwClnGb%2Ff8J3LyDL04n%2BB%2FqebR5YN5NEhQlDD4Wscb9Fvy%2Ft9LuKjpmK9PPtoGgEwThhgBp2JGM0IuJoM7JSrcBXsDqsqWKkAPGoi%2F33beKJGRPQPL2Kz7eRpCXukB9hH3lfbH0ZxTmJqL1CrJOwbAwXJLkdndSNPWr6asd8a0RK2jOrctus2Nd1%2BaAeiwTh33v2OBLPNnDtMBby7r2uxNHpI5Jv9dLMJfOSiPcTBjN9Pvv2h0twqdvWCJ9mRXzENUm64ExfAULyDUYl0hOvrZ%2BLsnNBmfn2ZR2f0EcDfqZvpDrYUFuR5vnGHo3II50%2BqBCzi3551ty3e07ex0SucKdA2YOAckxPftFQSoABBzbSQaslEx9MFcjVbawYtWVuLq39FGVG2gsd2cb9yvlneFIx1ESrURSwmsLIBFtxSUD5JF04oO3rduk0G0jWlnP2Wv5Ow5jn9xaVjjrp1oIhcCNEhidSDgZCLjCz0WkQ6WlZIKpnTehPk31u8ED6Hn73%2Fp%2B61N%2FRJyk3IWCI7uDw8HkwfwvBRqdhw0M74I6aCVDHMooSJoTh4pGrvoT5JEPib57tLzRHoGqo0XjmAsAEBisDlGNpn27QWOzEmuMuFhE%2F5CBokOMIHX1McGOqUBs8s3OPXQ8e3nDmhcOcOD3vdr6xJ6Ch%2F3AwqSuiKvMNNlb6dp7VhqzOc8SyRsdXa7exR5gHQrFRQy2HSCONVnbn2XosiUOPJD9v6JH4IsQ3KZYE90%2B6e7aTuPuX8rkZUBscC%2FzbuS7OYodByLexc1XogDCNWZfBywBmEqAPBTxm4WxQOCBvKKeoAJITV6IfVq7DIUSuviGgIlHx9TCMVZzMVNxxfQ&X-Amz-Signature=36e5c53772f55615c6d9cf2300682b351e563295bfe2d5647706506f5601bf6b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

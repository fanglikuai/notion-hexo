---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SVSZWIW7%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T220043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID78FlPBL%2BCNZT2Cq2b24csmt%2FL1CHhm4TA%2BWFVhsAyfAiEA2caWhvlKyg4HcIYMR4mqFw%2BrCNj1flSfJFwzxigdZCsqiAQIpP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCcI7WENX4ihWw%2F8RircA5GfIjszmnYZmLR44nP1SRq5jcmQlz62j4uT60%2BYbXPTFKj6NBROKgxV22JS2snpNR7Fa9yiuB5VVsCibir83251K6GUVh%2Br%2ByMtaXLYKwSF1XE6%2BENKDSYB%2BSglPOc4uenpNogUjFpXDNYM3mTnh5eDvmYeeV6CKoIOY6go8XwohqHmnmYO3cgU4XnC6OrYyF%2B2yIsDs%2Bc3HXB%2BZVfsT0KsrtbRA%2B%2F59cDq%2FXBona%2FHvt8qVzaNLiGvQVawpR9bkNPrtr744CONjG%2BGWobiYtyAm8Ax0e%2Flyh36IwpTuKxCtaogcohmiF%2BO1x%2Bqx%2FAeT2Q8GDnHTcpexpJLceeAjgoxHCfTCB7V992Etnh1yyjMdyiXtzGX7S%2FGlsrxQY5Uzt4FzeLSwuYtxjtwqOgAVFnrnwTRbEo09EADjr2nSC8w5jLMkVgvwna31HAiifZZO04PMvBUYW%2BRsdbUYQvq84LN2XLKIqmlu8IQM42jgXO8QZoQJ9OzFym0d7vOYjyKgeMASf3xhp5SplcIf5ZhdFLzb6LtPZVRPDPjvnmrqx8DMHuipOase9GQ2JBQPJObqER5JieomXTU8pSPgQB%2FVjd5ISw0Dnzyi7jG%2Bdpg7X9TThWJ9zlDSMsTDXU6MLW9oskGOqUBna7RwoaBjqsWbYdWyPWX5MF4PmnFNIP65RnQoFkFujzDTH2X2aqyozVqz%2Bknzh7CgzxXx4uqZKlrlMWNYuJsP86jOUjBiD4Mb4ogUf06z5E4ZPVMaQuUVBJeFZ5qt58VMcL4hjlQYjGgUi1BNHb5t%2BPYsK1uU8UGMY0MbD5mkYPkGEyk1x%2BDOLZJfx0jmk88xc716pgTMoku66Pc6c0y4R65ADnx&X-Amz-Signature=1b979b8ae5c3244821a95c4d435e3a467ee9454782ee1e086ef4df9148c4c574&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

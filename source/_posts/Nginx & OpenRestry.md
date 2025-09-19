---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJT7OWTO%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCIQCZQEEWsUlnDIRSni%2FRGGcioqyEh%2F4snDnoM5I3H50wQQIgLJ7WWoD9s8mNdRFxnGEo%2F6iV%2BC6yxmWiRFdMv6X5H98qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI%2FMAqET%2Bj3gRJ9RVCrcAzxcHqMrXjvHXFi1tNbh1UMRBa8346zaEH%2ByGPe0isobJHELvcLobD3Psoo1MjIEXKgSwdYfcr9FtsJGTcZPzYMB87ZO288HJ62ER8O5LLisdjVDjD2mEO2nq8SdSLenHEy1PS1Fyna5DnBYDFfOm%2FeKClXsUobtkKtJO%2FPNccr27JuQa43fgIZ19Wl8xxlB54dm3bAl0GNf2RJYuVH3djclCTqRIRLf9zvjPAIir%2FWUMucDKGRWz6zZ%2FkAXNRcufRtPYNoygXUTHRgumHDvvF3JTd6hYLNeHDCjayRa5H%2FkCKneu0Vl%2FzulrHUKDCCUpMeqI0o0hVbq%2B1QsGrFa%2FyIkZkWnFn0Xh%2BJ5EikfpKoLxVEhm90Fo3jDliya3BxrDKDirxLqW85DrXoCKlMuqtyv7xmzmJAhzNz9Az1zwXkjUtYgl842jwtTXzB28EQtglSM1derW7Z74wEWQgeR8WNweU87hDhmZL8t%2FhWS5n9wJRsPi5SklOkAvHViEuiRmgnp3tFkYmI7%2BnCoM%2F0jL%2Fnz9py7%2FkhnKlsXdVbq19DZjurA36I8WHk%2FIZ0Gez4Mz%2BVi234d29kqzqxdVIlQsxUTjNDl4h0gqTauZR%2FgaEJAveAFUenHgzCLM%2B%2BvMMm5s8YGOqUBkBkIgNecbWJmWTTStw7vTTdTkP%2FU5fECv4u7Aj%2FKX1yyvhcNreb1Rqo%2Fv1WwqnfIsuBp13gk2rlt74K0M8Js97Dnf0gHHLESF%2F4rZdebLCbp6h8FCK9W9BFfLS0EEjPgm%2FM9TTdnqHEmvToDEU7sMHVJUHcycF8h68PEZYGSbtBqyN4I6BG5MuZG6R4yLeDwaxV5J9shxb6kGlKZZM3i%2BPs1uL%2Bj&X-Amz-Signature=dff0e12387959dabe4923273de410b63f64aaa9685106b704b050b177df97688&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

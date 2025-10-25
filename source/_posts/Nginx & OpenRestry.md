---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466273ZX6ZX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBroSX15VoP2IQLTOL6PzziDO3HQnEwSlbE2lfkdjVpJAiEApUeFZR%2BVuFuSuObxkArreILjWVQNQo4R%2FenH9jSLTWcq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDE0lTOZ5g5ho5Fwd9yrcA%2FnykHaB6u%2F3%2B%2Bcs3Fyt8yeazukZT8oIU2dmrxJ9Yf3ByuTMu8YGmZV%2BxJgcWIOKFtZ07eV%2BOhspSLdLQ4Bzs8AG7K4VT4kxBYgasF%2Bf%2FLLvKmuoLj2QiNWJBBcoRBIzfr3q4sGQDa3ZeMrNg3iIdlPZuOOlEw7XM%2FRR8YKqcjVJKtkAxBhB46i3QVL8DcLGbIbbMZNmOu6dYgh72MpOl8OY44WCfgmRIVmO1vm%2FgSlNfgthDuJbFWNGU%2B95xg1C5YxxkEEGfN2wbq0RdLiiN6oJZXthemInOu8c1XNbLmXOiilvp9KtnyW12RuU6kIL24PeYEgsiqs0vdnOSEYdtRNXJ%2FRXMRI9%2BF7StPCt4gwVpVZBNPxTcUfckgc1PZrrM6j9dFXS%2BKrQqSPltf8s0%2BFIrUOSdwOg1ncmt%2B5oc9mkI%2Bj577k7%2FTrqP%2BqGF7ENrnRsdTWb0QSedyPxan6TgC5C4vivgEQCTHUSAn0ckfTn5slYtij%2BWG93%2Fpcq1kjxobM%2B08foCoEPSM6zPeyua4Evo3NxFrgMKz96ehRzfcyBx%2FdSGBj%2FQd%2BU30wVzKBcahFLYS6ScDUSxANJahWXmKGUmbt11kfWEp4Vmtg%2B99WaV%2BmXDIA1Q%2FwGvPEdMLmz8McGOqUBu6KCaAjAR%2FCP7ASnU8A0yHI6K2A18YoX5gFZ5Og06KuguSdSu1LmmxKsZSj6oikyBhEqEFfVYIQg0eDqegPTE5q%2BG%2Bbb2YqTWrL7OtuelG4%2BwFrgudWzmsZvnAHI8q7PRpDgRondNlAHpnut766t0TQoC%2FoYulttPaFpH0ut7h%2FXhYZSQSyX507KWtv%2BBESBo9Nnj33DIvPLTOsfnfQsY2gkcKG2&X-Amz-Signature=de2dbd6795722350b23efd76a5f04af4bd9610123b867428d592e2eabaa77fee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

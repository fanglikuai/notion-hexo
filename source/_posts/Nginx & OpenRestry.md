---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666JBWM7YR%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T000038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCPi%2FAlz25wBOmqyBl79RubGb%2FouSZ3xq43eS5bNtvPKgIgA%2BdXgB90dQmNTgBDpODjZ4jV0izaU4VEcoMkbJx1m9Mq%2FwMINhAAGgw2Mzc0MjMxODM4MDUiDHPag4LjLxE2XthT9yrcA02Z6SoEplcbFzrdhYRuAKqn6MoqkO1t1QGWNEjj97DUtJhUMhQ4OU%2Bi8whQ8k65PyP%2FvalPLUpYXnbDf5ndNup3FG57i%2FkOwUxHhVow9bQVep7uA6%2FwBdlk047AF4hgQ5GuZ4iZ7JMu74W8Co1wxLBemwYFTqanc1ABtAuLfYgM0E8KTM5Ujc7U30RfQQMYiU7GYwk8zOkA%2BvLDHGTgVH5t7rp60HK1qUTu9ZT%2F0WEtFf3ijR1Ra8VlJws8fevr4GurUNhO%2BKYxp5cZsxrjhF6s%2BB0DMXaI8YYTB34JzIy4J0Wi2ghfw0RGwVBIAYeW0JDML8gFY43%2Fb9x2RCL5%2Bc3iP6bVg0c7AiiWtxC51E9I64c%2FHcyfN%2FAF%2BUvuVjsgZg1kjtU%2F5jp1ChbKvKG151ngmctGxpdKt9fBpOzcFsSkW83Ybppxyqt44vlKhvtXDM0vyn2Aleb5UhqBecnNmjcq1s5FUpy4mdOA%2Fck6YzwOFX4Jww9beRur62%2BmIx92RTq4qa%2Bhxoh%2Fp%2BwyEnnvt1sUJ2geumKBd%2Bibk%2FspOes%2F6A6QImC%2BXbezE0UFFFRKLQu98RZlffWIKUTvtZdMrgrEjm2jGsrcfWAxT0Q2YV8OtZ%2BwibQ0y64gjv9MMMna%2B8YGOqUBwtEs%2BCRWb524SxRWZVIesqqut0evZROAfxSlZH%2Bkfugw9763A1N43AKCgPGgKakZ10L2cKWGocZTPaejbQQ2xVoWGKUd2mc3Ynspd7n1Rk048JDUkWmyyhQBHsLSSx47GrlGwdWRFT2yF8AIVbwPJsfwp%2F4zGxwSwlOSy6lSHQIxt7cc3Knm%2BSjxzjyGE0thXU%2BtOK4%2BeXOewUl7YgN9IUhVr5AN&X-Amz-Signature=5c182443a5121d462fa9be7a85634a3c2ba47cda223fc10f638baf75e817ed26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

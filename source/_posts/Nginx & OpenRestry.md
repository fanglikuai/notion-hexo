---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663VT4KNQT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDGw1iFQiF6b%2BzhdHTOKEvSzBBLGXNVfscpW8Z7f3VlNAIgLdTiitj0QemXNoHakcRlCDWEirCTyHKDAkLJNKU4AVQqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDD8oo9pAIvZ8W6zdeCrcA2mUP%2F1L7epmXAAg4t3mNMzJk%2BHIsrA%2FeV%2Bc15seULjGetObY58iuR7MUq6cnsKWiK4mbOqu%2FzYW7naDF%2Bh3nf%2BNsXuuudJI1OukprxE8QEHwF45M%2FH9DmvYBioT%2BRsgjpudRSzdnN0IRszSwQbDD3JGRJyOydc%2FD05v3A0LnZBUe3hztFktjhk1Le%2BhTqQ%2FkG%2BIU913BmkAP0S9%2FNAq%2BtoRoE3bj%2B5v%2FlrM%2FB1voFXaJZvQVGBr6P05zRHBcHOA4X1k%2FPlp%2B%2FbP6YiZ4uprK18ZEK7yEsmRL4%2F4A5BCOezhkynKPuaCP%2FGzryMMrzJfHW7h9vohZrPY441S%2BE1NAnKTIcjbAaO%2FQGxM97Ik14mOKbUWbDPNl2OuMixupepM6n%2Fcci2kPi8JKsKgtdo6IFYxnTNEf8cl1ZeQUvVuiXNPCsxRGUtIxVOzpsKwXm96MU83KUke7ZQea6%2BTmKB1RbGhD1UX851tijvQzyYly54ImLU6Q9grXLxmdNiTK%2BeTwSwftXj%2F5v%2BCmIxjolB953kTKP8ucmp1XTEfe4qC8Drr1XPHOLdaD0vleqfJ9JKrkv34GMNtbJyHLim9jYHL%2BX%2Fjqgl2F3RzRa1vpx4Nl5e3dlQCx422KXEk47WEMP%2BNjccGOqUBpfCwuDsssyxeUhJF8Yr2APhsQBtoRZcV2W0bk2YxwMiIQ6uOMVN8ODHXSjp0B71xqrWPqoS%2B20wk4pG%2BgWJzZOG6gWOR1PDS3hxAuNa1k3927IjQCLQJkiOqEXHFQUEzQsCzaq%2B2ikpAd1G0i9WOqwjoeAYNrU%2Fa4ukawGuBI0oJxrHENNMVhZtf6Td2EhxE5N9q8K1gGJmKimu6sUDM%2FTEnnJ8p&X-Amz-Signature=86564e4dfef6b2e3caa055d890105dbd37bfa7cbf9d4cb7261cbce1173a713fe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LCFYTLE%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T230036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAaW4B1q1%2FFzcSmoppxq4Pmcg0Xlp%2Fkd1VRGwcrixSNGAiEAuE89FeAZnyoJCc%2BJ0xSDGJL35PHxBym1u4fSeUWla8Aq%2FwMINxAAGgw2Mzc0MjMxODM4MDUiDFCosLVTycVCrqeG2ircA7NOt0yyNu2X1gJtk7aCs6DdxOma9RxpE05YAJym5uHBL51rGp%2BbDz%2BXO94D8xOIikqHnrV76Tngt%2BMOp7z0sPTclA%2BL%2FF2O1XmkZ7MxHzuAXOEtxSVOHb3eYjcrIWFkeEHg%2Fd9Tm3V4uiOhM%2BInCETDrsG%2B%2FTpParqP7avNP5OSny5WZYmZaCIbMPE2V86O1Yf1XMB26eY22Qy7d0Ho%2Fba2%2FlKRsoPKf%2FgObqbX6HdHmvTHdO1lbfH6YznbQk3jnD8pr53ld7X7za8pZbnLOS%2FIIGpWHSqLAbZ0b%2BpYXWpYi6WCEVpbJ5WdJSFcWAQLlqxSR0tqEPDsCb3WyLllN3OJMdi1sbt2nxlluG5cRLa5ZPbOEs7Eug%2FZlINOZkI%2F3hpLm78Cy5f3pbxQuRx57%2F68eRoh4iwpT38zrQbzD%2BEO%2B7Q4Aw3wkZEeP%2BY2GzNWbHw%2FxH4zzg2bAYDrCejRXMnSqVgu1zXbBuR6Dy0a%2BhTBxdjOhI3qLccnX7WlV1G2unTrACURXs4eWEC9iQU2LIMfWx9Y5huv1ccUjoBu06wnX39AbnwIDwYHKL2O8X%2F7HtChD%2F60fEq0VbN26tJ6n5ffWD6x14g01%2BIAHZl7IObkcI2Yf2%2BE94iAKriDMKj%2FxsYGOqUBt699iEHh1KvEC7oSGI5iAcIF4J1NZdgih8kkaMyVAd6rBBxzs5Pb086fU6USbBa0uodkCJhGeF%2FHm3NJc65aqCTR%2BdJ0Tu2UbNXP5yyIriRggtrROqo%2F7lRFCktF4qFwWY0B%2FwCQMyer0nS0pSkRFdsdXuRCHgo205r5Xag%2FnEuMcybUV8HINFyMd7oPYI4vjUtCdLSWoS75Vj%2F6j8fZ9PzSeUs%2B&X-Amz-Signature=e2140590f2f11dc84330481adf478526d045ab4b98adcf676be117475f1b486c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

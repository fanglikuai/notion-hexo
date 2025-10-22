---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFEPRFWM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCOLUeVoL3rEdAsrWq81BIffmnQNFRQfWRamrME7kMlTAIhAN2mrzdtgw0g9XmK32W1aLFkt%2F3q3Fnm7A6msracTKsOKv8DCC4QABoMNjM3NDIzMTgzODA1IgzZQ4KvGmp1irPWTJEq3AONfjAtxUu%2BN5J%2FVKJsJ%2Bn2jeX6WGBKBedqR2AXa%2FbpQXH%2FIlLTGSKOewWra%2BZ%2F0RnuGohxg9Uek%2FBbivHIFwTkfRuV9aKLYt%2FHN%2Br%2FHkSXocEcamdA%2FVSNg6RA61rs5UYOA%2FvfdZ5Bb3yUVDeuDwQYapGh6kAXgr3rSXczRJc874XlFDcnT7Zw4Sp09Vt1EHCrGdR3jUljoOVy%2FH%2BYPSJ%2FZLWgbAF%2FDXpMkH7m5tdkh2vIRX41O47wAfYmcPGkPb2SJcifPbu%2FxBOGZjH4sPwVxR6U8H6%2F6oSuJ9UESSNZLJQ2f2v23A1BZ3jU%2BztfDwfBW1HLFaZTjrpnOyq1rBvFMvyi9sQCXCWcomoTtvKDPoEE3BmY8JAWgbO%2FYHUAJ6Y6dHEC69pthzWuaEPG7WD263ppuwcN8raEpYI9tTvLdME9WRqIKq5wA%2BkbPwCLhrESqc4bYLl%2FdM2QuuXC7dLwO4CLuRPRJR%2F2Gy1y321GY%2B88%2FUhjpAbvIGnY5DMRNcrVw12xOv4pX9EW9GvjE%2FEOL%2FZIGO5YDS3v5gUGQx7f1Pjv%2BAzf4XjHm%2FbqQ%2B%2FdZ%2Bs%2BQ6paJFo36YgO8meb3b2re7lR7IRXVTEQxGsAETDeIENSwLCznGYceB2NczCSp%2BPHBjqkAX7wMUaJMJw%2BKjdTxS8%2BT%2FD4W1UOLFjLDrNWdz82S10G%2BuCTVEkrbBqmr14A%2BmgcIs0iJeNn3%2Brdc%2FQwCpkRvIXBHywpnUs5TEiwJ70nWPChLkm5fDzlVrdvivuyZ2Bbhcafgw6TZHKIXMw0XjBfBTqpeNAnYA6gv%2FkeUrRjeH%2BZpDakHo1diHhVOteAuBl0FIcGBetuYbDDa%2FCEyg76as33ZhBT&X-Amz-Signature=936a167c9a0c34f67a2ee05ca0517cc30b597ed86810763d31e309a38bfb104b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

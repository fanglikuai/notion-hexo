---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XT5WQ7PC%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGcwtBm4m2rzNxRTXmsa5a73bf5yICm8%2FeXJlCohyAe4AiEA8ewWGgSoUQ0Zcz21J6vurjg%2B63vc8%2FMG0cHOGqc0byEqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHxH3oBCpRDaDgCypCrcA6gHEESsMZhwh4b880uCF%2BIVAP2rAD1u%2F3kcUb%2FmRE3NP6LuY2RQaH8nExhAN%2Bo1iRUZcUFRF02SWpXDTZ%2FaC0gd5F16ruwQ5toNoCRwMWBARo9rYDood7l8LSUsRCwOFnO6Y4bDQ%2BdrvefWvT3YwdLZVP9M3cB27UBOy%2B6gzsfDNf57RUJ2ODya9p20jw4JRPfOPc2pn%2Fer%2BfVz6C5gcy%2FA976f9CyZIKUuvhhUtj3HhLe4a8ztt%2BsOciHGFcE8QPIgLIgj1xwUm%2FaiqlPZzovIAO7%2FOkLP0LXO9BPmNySS8EB4SlbS0JpcXzW3uTmlIUdgPexgPavFX4voXwQaAhLU62haiXbrgB5xiPVu3ReElN6OnD61A%2FQEaIPDMWfafhkEKLHq%2BWVWzGetbFBwwH8WbNZl73%2BsGsBtJY5h7bNZYJcO1Q6%2FIdJMuOtM7RccrKMFEuEOjRghvS30bIwvcBpDlKjAbSWXAUj7orja1fl7RM%2Bvaa7plmzcRZGakXi32fADMZ5WOHe837WHpIlOD78siPZNGZ%2Fc7hch3hXA1OfGSVUXWRtjqIDFBo0GBKeRet6Rwq3XBnE%2FygXKqPEtOkovxdm2H5cgiccNxFbzSeIa5yljRJ9%2BevLbZSbGMPmQt8gGOqUBuqJ%2BdNhvy0QDrkS2l0jucC626XA0Y6qEZSIrSUWdTM677HCL6nGTT4pLLNC6pqvMTyxuai3CPebioW0jMGzFsZ8RrMNKrJwYJ16GgbOViECbIa%2BTGxOXtoU8375tHlF1iZMIhW%2FGzm2epz7494I9m%2F%2FEU9LjF6YLkAOI3UbT8lnqwKRtnx0iFzax%2FYmsjW6y40tvTbX2TAKo%2F8A39Xs7nKFaN4pO&X-Amz-Signature=38268037c1f915f7002ac511901b60c12c6c9090fcc9a07ebc7fd2fb73b232df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

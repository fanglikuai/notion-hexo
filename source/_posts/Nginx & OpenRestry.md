---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662OKUQCTN%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDaTj5avkVnCiEpuMu6r%2FQJ2AgICsO1LMdYp%2BcJEUEHRAIgB7SND8kY58V0Rs4uCZejsqM6kevLMdA%2F07zyXOPELn0q%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDA2Z%2BTKPabZeAKLeeircA3Fa0ugxzP%2BD6iQ31rfgkSaN9CBwewslCFVgy1TsXEazu8%2B2aCgytWE082N7JgdzlZTUuLaTRmAqkfe8J%2BWbhy1pd3VfLjS%2FAfOayy1I3MVGZ9W617f%2BfmyLS5thFLyhCZjwOzZ3MK2ELW%2FOZCtWMiartc06eWKXwT6boYNA%2FIXUTLug%2FyrLRt3fcdGL80Xpnk615wV4fSlmWNFZ%2F397kld7Sa3yF7UdrdnyrQwGonC7as7MsP%2FXwz68soJRZQwFEYDarsGVJRjHr1ubJNz%2BMKIzuKdfFXCbEaOjL1PRGptv9E%2BIUUW7wn3b65Gtf1zTkWlOuq9w8jo4io3vGK12czjLDx%2F%2BtJxjKeLIuGTtf6m4go8hMGRP5lDl7YYbSfyqb6P%2FjIoPKLcUbHAwFYfWW1TCXSpDR8pCtIpdxnD5%2BrIzpYVGjv5gDMUMwSxtSIvA%2Bf6q0iz2%2F%2Bj6RQv5D1cZcju%2B7HD%2Fgf59Hwo6wLH1QTyF32dPhn2cA%2FgO54P%2FCvUTogs9MN7EuiFc6LWGoYmF0ha5viurOlPsK8Z5M4XxR8OyDKdrQv7oyDdPyn4Reo78XjfOOdWJG%2B%2FR%2FTeICHxtvWs45NAtX7ERb7%2FtZHAy4Jh3WRPArYdG6T3o5Cu9MPPhxcYGOqUBzzk5by3d6ddbm%2BFTnUtZMm%2BBQhMvE%2F8fD%2FpE5xeJt494BXTBembjgVnBkmDsLE9YFXkt1AKbtcKLIsKySeyu%2BNUhGJZOgEZcJR0rfhIGOG35OhUEqrMVjdeolpIX4FUR0DPNmWLFOStfum0AEXOlJ1H5AK8V9Buzr6Xn1WOEpIcWa%2FAYQM%2BSzV2QOmbVWgDJRq3GsuQqDlBPMgkaCOkGF4AxtoIZ&X-Amz-Signature=f27c2621cc4bcfa62269cdc99d47da159a140fd3e17c966971d6007d444b00a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVUN7G7X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOswfpNhUXYoLUN2x7HhP%2BFr2d%2FiuZB2yKm3n7YflXygIgEqD7cfpT%2BUu07xc1vDDoEVziJdITXu178bAMLmYCmygqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA6Ib5cc%2F2EFPhBbqyrcAxWKJAFWFz0rBQl%2BadK30Lvw9kdrT%2BIRwr6SqoUqbPU5rq21cxxwv6L2Djg5pgy8IvtDPqNmAyK84NDPQs%2B%2BVV47UqafuM%2FURLfMgoUkaE0c%2F66V12WJtWOGVBGBdc88UwOUJQ6g%2FTMIxYOzMO2AT4bjoP9cD1AAfHrAG3T8SgmGGRjqZ8uB33PtpdIgDuiRtDfSPFVKtA0s31zWEalQdRwxyS2eBR8GLc%2B3X7iF1Fd1bx9k7hAkbq4IywRnvqge5VguO7ZgFoOAX0f%2F9k8OPmdjaM5E0Q%2BI61tTgpWU4nLrknN%2BL9HZ%2BUd4doDPKVcJqTrwCqCn1Xl9c9viiU0D1qe5XqnC2PyktdEIk2dpPJRpA0EKfrPPKeT70OsmwpO3dOYItSzdYbG4Pag1dWqedtiG2B72CEQzO3GrBqQO%2BD1gejNjksnGsGy0LtTNr52VZLbvJFMK7wDL6UNmAatM5TXfkCaLUV8PmmqqvaDStYjuQ2Zo%2BsVPCvPUWZVgA%2BvnYr9ak2%2FNifH8w2LS813TV7ad8VEA6Zptf7JwXs5eBaqEtTObDpScBlFFA6robRbmNK03Ugi8qR%2BgqBKI6Fh4UzTlfMXTrUHcH36E36XnIoavYnGyPV7v6A1G2aiYMKja%2BscGOqUB9RzySpvxwdpVblIQXXDpwUqPrqq8scxgGyonjoCl%2F4jT5IwdAY7xe3z3TUSr1zDo6%2Fv1VKvNviWS%2BR7vehj4ub8w2Uy4AVkM3Xfxnx%2FV%2B0o3ER%2BTAUKa1NRVYsAoDc1XuPPh8zSe6gZqm1oIOwChYpcKa4Umvg9r0jIO67e%2BFMtKkEkIA1Ni5VQNd76gZiJkUokNLpB%2F2yQ80UA%2F3%2BhtB6poflD9&X-Amz-Signature=cc0864f7fa405ab5a6548ae519392ed2d65f0287fda1fe4742b461504e3d7bce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFLCBMRT%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCGsUeCXl4%2FAm3uEZef0xFWzp9Z4Z2jTqOMAjrViVRlZgIgQ%2Bu0LNhVYKSnxiDcS1IQR0v%2BZX7Sb7uOZ%2F79CCFVF5Yq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDMhUtp3iCNtsxdUJ%2BircAydM4ozFaywhs%2FkObDjm5T3%2FK7oZM%2FibX6r94arFs6pNgOI40zu5%2F2%2F1aK4geE6rrBE0o3GMZRJFgz9SN5Xjck44C%2Boym4EfRbIzxoW5TPBtlvHtfJcqDW0mjIdmBookr1Xli72n%2B5wVgMRJtNG%2Ba1qRS5VjCF7DCk0MH206Bo%2BfunAS1GwasSMLnjxNmfBwC1z2ERWKbjyStv0%2BHLHrdjFuXFVR83Mh8eFo1u3hK8HjuCjU%2FxE8ErhUvk49VCAuyHTC9hhUciuMTWdfQpguRaEF1GT4%2F%2BTK5beiLJ4sRkZVCOuKb6gKtgOYcV49ocB7tdG%2BHk8NZ5le7kpEp3pqFXP1AcUh5%2Fx0HWemIXl5c7N3RXCVvZeoxIRX0vtq2%2BUyoxdKvI6SYbx86lE2dnLAcQBHRnlLB0v95qUGuDrVEaqEH%2BLxYeZn2X%2BbwI6%2BWTAOEoGt71kGzjmh1u%2Fq2MwHX%2FluColN2SuYfnb5nKTXmohQdMAEUvf8TPcRMihZP0AS5yZgZ7wXR3XIc6X9fP%2FkLLE2TPiD6qMWLEOIAMoqhSQ%2B%2BpcC%2Bi%2FOdMdKCAROXoSMMgOf9lDy4IKd7E%2F4GtQVU9R4VPd5w4xZAlSBNWjJBF8rCnXDOte97Zn5KtxIMNaXi8kGOqUBQgeIdgbEgm02TY7FmJh0cGbyPvdvtBS%2Fj1J59mfIWBK5Hmu4N3UiXhevw%2F27iKVblisDtR6zGLc0AXZ0nt7ERfOLPzZQOi1J%2B7T%2FyZdN%2BX0Pj4V5KKlZLRsvhJOHQGPOdiRBcUMBcL2UmVwcYRh%2B8lWo6zE4SRDCZpysuRZHMnYnQqy4XA52%2BrdfaQqrJ%2FaJr2yuhltU9bxpqLmjbfpSoF6kBoH0&X-Amz-Signature=1df47c9f4d405405b7562d7576beb809c25a9efd50cdafe010f8c8b2436a8261&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

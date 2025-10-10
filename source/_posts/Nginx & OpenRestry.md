---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RRQQIWW5%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T090048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIDwPCedgZSa6LDQyZbdPZ%2FZlnPxaUpspkx%2FIEi2CDwDwAiEA053CziBvsxupxkkWvviwa4dcsqnoFf0EyrUysLT5hCYqiAQI6f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPew0q60n3QCZFSXIyrcA9ljNMIVlgxb9NeC2z3uSfmMYRmAKYiabef94kQZDVDGHL2fMwItLq5%2FzWo%2FYf4%2BudFee9ABNA%2FA8So4LDlsm2ki%2B4PGQ%2F8B8W%2BnmvG2leyHhqUmEdDYqE5Ckj8mX2cghGGj4CGhmmw6vKHJIeoHYzHoH7Mea0sp0pckEqzUNuvyctBfAFqEg4K14JdygHMCJ9q9wwfKf%2BGeSQaiRKRrqniSzBQuh2OCWk5vtPpfWXih1lhMChoWj25GNiQc%2B3dIglF%2FlfpDK89sl2vxgHqxt4LCcylpHoEjs4%2F8odxR3RfKNMvVNl%2BZUmkJnKp4BR4Sk6z7OLhlkl%2BGzHhxU7NwKwXcuesAwxrUQU9t3oIe%2FThHzaGwFhs90TV66TNYjlb%2FC62E3O5pDY5AatpO5mZHo5AzZJlGuHBjVCTw%2FuKZ7fBTqocLpx1SFPLDn7pnxb%2F5saQWfIV%2FzOfqBvNlTjkH1yBC77yk5dTAd4w7tdaNRz78%2FIHlQyu7vgMYLfEKljNfSJGCRShCzwEfD8yGWXz1IDJd5L%2B4mlm79u7fXkbMKT0bUWBByqKz4SIPtD3DyuftcMkdLl%2FHorLCY4YRCLUPQW6GNry8tSOkzWGoZCViHKntObMzB%2Fy60oCZyFNiMMr%2BoscGOqUBUVwWj7%2FsXo1CkVdrp4gJ5fEWbJ1EOdP34bMjNXw%2FNnZ%2FH%2FkZROg55tX7Kx1FdBY1m9zp%2B%2FSHIX45XffjuOiBVpR21whtW5xYWsnLauPqytNWPUhg%2Fx%2F3quFh9GXlAmNgnDagXMSMIDXnKAS5Phd77rIuBMzgxAiYkbj2QdWoxIl2gOFSWRjfjF1O6NZXVB5ENKFFseddEVWYCIuRhNr9TSMD%2Bz%2F0&X-Amz-Signature=bed0181c36d9095d9d6593209a34ae4e44e5778fc8f6b36bad4f5d63635346b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XT6G3Q7D%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEG%2B7y%2FoP22FMK76TCfb7obGSEjDw6cfJcB7qb%2FvU7kRAiEA4Tldhswlz7olPCQPN%2FiVPz3yMWVcC87%2FHkHXkYUV208qiAQIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH1xiNqHn82DjathsircA1JzVnph9ZJdR0CgPnV%2FEXQLb7GSMFFzLpAMLLkb54OnN7AfMxawPzSJaKTtFVjPY3c%2Fep%2BG7UQy5%2BDT4%2Fs%2FW7puCJCxF1z36aF3InaHgTqEJNRz3rZH326RQQvMjQ8p4gUeF3R07VjsK0WJ%2BQ%2FURwKa6%2Ba%2FxltGfpKsmRJsA5gSXGjHjzrhmwBV7gYRe9vMxhtiT1%2FYuSUj%2F4YGk9MaKQpbAINox8eXkRwtLsxztCqiyPxFcUbzUdRXtW0MqqwM%2FlF1dwUyteHhjBOPpWDrYYARB5TX%2BVORjceFNprmLerhZg9Uq9x%2FmqUc74pGih%2F7PIunAZdaGIuirycz0GZB%2BwRkU4iZ7QRmlYu%2BWiktH7jfSr5fX3Pbu%2FS6Udg892lro9IlCWFsUzTSk8aqy59WE17pvwz2CWhHIOEHre%2BQdKPeQDYSOXG9BWrHVNXOobhrnCc46fH9Z6hgx7HAm6NqkunWECHo1ywYxEtSyPQn5QMfdxfUs8EjucuLcXG6zHCeZr9yIlEcWd4Fi9tQtZqQWkoApwmiVqYeWctuWtUvgACP4GygO3dX40NoLLHJ4u0jETRiKZwB5Iq7yrrOoAWptfmZqLL3PJ6w4ClfmJfAkEkurhV0kmtOcuBgnd4aMMCktMgGOqUBGe1yMWdulZFe7%2FiVWnpYLMTtyeguQ%2BroKRS1E075dcOwDRKJhJ79qKmuuonk3dQjEnTN6QpvwaiLKiuv9iHP0%2Bo3e3hnUQJ0sAidRiagocXbxXsgbq9TKEwyAxQ%2BgJ1jU8h17tYYdzhx26NennFcLkLy0eSMyAhiGGbkEQo5Bs5HaGGcux5qmERbHHmC8xVz%2BtmiEcTfh64XnVozqLbgTowmLPCo&X-Amz-Signature=a48c5df0717cea338ebd79d6579568cf7ab02d6a471d82c5fdf2e99b530e9e24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

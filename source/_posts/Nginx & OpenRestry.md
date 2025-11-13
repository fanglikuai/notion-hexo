---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SY272JZY%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T120049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB05g27uq7xGZsn%2FEinmWF87%2Fhnz4pf39u4Ti9dH1bTLAiEAyEI25pU%2FHGWoeHbHUuu1eJL42s9838lj%2BmKGGwznjvYq%2FwMITRAAGgw2Mzc0MjMxODM4MDUiDPrvlzXEqV1TxUCr2CrcAwZigrCMIdrVMu2QCuMSpCtJlyxoFAe99cIWuxGRNqJba9Oaw%2BcaxwkuZPhoYj7GURoT2njwEpMJbxWPE8TLJYNzwEtUjUhqIc1aSixLaTRzlFLdtgko%2BhLQMuypCYxvu1o2FqQz850j%2BSwAEu56BOfZaKsGVqAC3X6M%2FqNbuO2JwXdO%2BnisDNbjgZT8BumotGn54Nko3ftk8cOdYMKndPrECdhBMzR%2BtrznpRybyJ1hTBFJQ0c06vba%2BTHIkJ%2BeCvXYz0qF7jc0uZt43riCRTy0il464iM8W8XzT9Km0SZYXy9JPtNR31l77165Bi7tGqQwAnDSO6foNUwIMaPbACsstn28FdPBUcXW3N7vqXu8PXaouT%2B3wNUV44%2BIfc%2BSKZG1PCfl8EccrG7klOm5%2BaOzszTWztHwyt7eOwKcmz1uqFYHvMQmrpFi%2F57VS06Tz05nr%2FZZuZLALGyY1qhS%2B9Kem%2FIsZpr5h9h7tcJpwMTi%2FUmrFGW%2BOMdON3QH%2FI23PKbKXztRx3VjekAMcTooac%2BORqIT%2F65iU2JezqhSUKTZlcq5DjryR6Jz60%2FzX7zSv61EvSYd9bOHAI46gQ6JDDubeJ9mj3j%2FLW6JKuHT5UXOSjfMQjCxkww0w5JhMMWA18gGOqUBcAVTSVgIttbKPxEct3PrvC9NEHs1qYTemWYYcX26JYqOO585FUnmbqrN6lWE9XrCZSKfU4hEtnH0oUluMgFiZcZV8OwE4pFah5Ff0y7wcvD1jYHTifZ%2BbC94ZakL6FSVB%2FKElPrXVkJPQNgjiQPJZvWZc9wfJT5UoKubM8YbSv7tvEgs6hUvLrriHfRAH%2BesBI7Lc380nKJ%2F9GDvxaZbuLdKpXg8&X-Amz-Signature=7e449fdcccaa864073af5abf652fafa8ea2abca30172d9684144b1e00c226d7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

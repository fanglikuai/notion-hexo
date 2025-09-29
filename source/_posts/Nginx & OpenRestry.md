---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662PT6LD5A%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIQDlY%2Fr10FR9u8w074i3Y08ZD7MwCkAhw1u8r5usR3u%2FRQIgC06UuoAo9pfnYDJfBf8j2HJNt0GjXfNDXcQByi6gT3gqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGjyw7HMQsCM5nCBlCrcAxd0z%2BMbGYoiEhWsU0x78qpntnWltvviOVPa0UMCJWY4SnspOh5WlB%2Br2TI0KyO9LUzx9S8z3Ju7yoDqVMJJLHWd%2F%2BB%2FgkSJmSvkFMRjmbNeLQelN4%2B0WikkL4L%2F39ne8VxsxuuGDF4D4B4avFEQo6oxFJNYWzGbO7XtuTo9nkShba6Cip6cp0BNabGzWoviJFx3nM%2BVnHKjTRtBXzGJzKGgOupq17wJ9FASvuNGTfLknqTgT2Suv2ezoVMn9j3Zr124E2%2F77MjNoKWQ0lH7LjFQwUgMeW7g6pZBxwS1wtqffuvZ%2F%2BDBtZPQjTVq0cmaTm0fEs9k1eQtBKNrtyCwZHesdB6OJFMxSp3%2Bs3%2BiJwdtWFpbYiILFAI4SFOyyA%2B31ozx7TuE9B%2FHdjEiVB9knirGgGIXE0F9LQ6McQUXvMJ1yyot6vyhebLkm5WwUweu6KLi2n5acHxjNk6mP%2B1fWe%2BPKnGGorIYiF2LVvKNK%2BKqDcs9E1cO36BXVmifeFYa3C5v3Nmw%2FnldC68gXbv4KLu8AH704Of%2F6CcnoiE2phXHyh4zvPyZPSTBT4y1LrM9W%2B%2FFmDJmZYywKk6pBDYzwBxEODULM4NS%2F5EsoIEG%2FwciYOadrtz70k%2BIZYNCMOar58YGOqUBdfwsTctfC%2FHlC204FeEW6DijeUV5T1Twl9XiYjvqSWe2pvwCwnTr%2FvTZyHUdMbnDeW5kFlelan252kRQD7DSJzTddjR2AxCLLRy6I7AmvjXF7R5q6CBB9ObZii3HcAOEN0%2BbYHUaEmICCPKFEhyZ3OCEA5pJ3HtTAQSSYbLA9dJsmFbSnsikfvGV4dYll0UMJTR4Jo2kIAUZyCD0Tn0exFXckUV%2B&X-Amz-Signature=8a8acf5e1d7253645b2db3147d9525491b363b3472ed96eb959640c653e084d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

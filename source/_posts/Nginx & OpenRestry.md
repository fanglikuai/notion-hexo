---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46666CHKNZF%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T010048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIB1udryz702oVhjMPeYiSXdl3RacXd%2B6oeuiPeckYORYAiEAy9OtTswi%2FEVbXum9qCGXfrlhoYzpdtPpQX295cpylDgq%2FwMIchAAGgw2Mzc0MjMxODM4MDUiDL9sOxklxSRalY6q%2BSrcA33%2B4Wzxiik9m0d5YO%2FQczEzTR1oaqs0LpLBDmAmvaCPjg8XIXVgDx%2FZIaZqx4Oor3gK2S93ZNs6iBJ4j4yPdl8Ou5rFKQPwSAMzL0SZSO7IwhsvRVse7tuVJPzQPlQ5PLIICb6JrmqXQCAasF5Uy%2FAtfD4hyl9LDgyiwVB5ZVl7U9Sb4UY3hfbuIMWwrvjr9PoHIZwFGeu4jB8J1sHereQaZvokWkGsLeRh21N%2BNXvyNf5amr42ft1uCzIpu2dwQb6T8waSP1aLgB4GPJriUVD5XrR10xq1jhZ9qNylghKLOJEYaY2DiiN9F7GJ6uwnlyEHZrtuMchQLfOkrK6yhE7As%2FbUSqE1zxaBQd7BILjrDZnwN8oIxY2ylFx4JMMubrlNiwVXfIEppRt2w3xiT9yEGXp3j7aUnBCqe1%2BmykUisJKDEvM1ZMcIgrFAVuRdrWLTsXI86t6iW9dxGRQcBKaTr346Ime0%2Fuzjjl48t4jEvMFtEdiWPU2RpOcecK%2BeB0vbCaoY415MHrWrbGYInB0ULvQSna9QmP8Y9Z38QlexDasjqOZTnA78fKcdOWIcwoBcZFfpD36B1zvDhbRb%2F8tr%2F0crrZ6JssLssvlDJE8LQhmdHzGRRmf9vXROMMKQ38gGOqUBn3aMSqHA3%2B7ZIoevfzkcoLfA7J3gBtwU%2BuyKeFZEzXfBe5MqPiYrtoTtjaa0%2FRD%2FxGCmipmEqApAa5WEZg%2BBdbjHrC%2BE%2FkMWCRVaWGikKbeM4%2BvimTQbTxaSeLFOMf1DOC34lXAqY259Swyr7sJrE34RXjU7g%2B1tQbA9lrSTIcm9piyjV8oe3TC6ByT1zs%2Bol3Ks1FEYPqVuBYWqNXKO7LdQnmKs&X-Amz-Signature=8b540c0514abe3de074d73a3180d1adca7115f709a79a6273ab19ca6c6345ee4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

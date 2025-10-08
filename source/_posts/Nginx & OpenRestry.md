---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46634FCGF3K%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T160040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJGMEQCIG0qa%2Fj8XurDEA6APBBck8PqgkRwF0rf4w%2F%2BJSKYXompAiAyOlNYcVjhf9RxB20yN78ZhYwB2rbVbcStmx7jAMp4uyqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjwPFYSfr0i0VmC6SKtwDxvr94FfilmJEwXqpIq%2BvOJFtVJH4qmljo5h0vtWKh0VaFtyZ1ZGx5Rl4zVv0X9r7NLT8e1brnqcTstuXMmQnpDUdVCV6zqRxLfzXwX6O2zLeU3%2BWl8HdCHmuLOpSVl3lL%2F03pEinaefl2b7qS4jOQFn1N5RqjdHVVxCNvw8OpmEhaPTBvvemprjA0PpfMqs%2FcDPTSwwpe8I4HhsxXJLeu94rtVWvHPtTH2Rh%2BoRCTCoJYp%2FBFLbT1DDabSCIfqpphhVIEeZ5kZ%2FLsDxT8UZrDFE%2BHWegyQjdtJKlih0mgb76ENqDns9EGF9up68jpkP%2B5yoYkPU%2BMZMRTcFlKnGZS67tEuRbOvj2JIp2g%2BfGJ9EWp0L7iNM5%2FlnRxnhH5mTJe5fgCuvnOZwVSu7HQpNo6JbKD34iWflCTFgGHNW6PiP5VCkUFGeI8rKT2y4yb%2Bx2yKoCfkWzUJp0x6QEL%2BTUfd5%2BAvrzAMPSu80DcKp6mZCuvBP19SWsHq8qexUPWM1I%2B5hTAqNn2y0aMqsnrSw5h%2FF0Wcq4yXJMioM%2BZ0sZfe%2Bo4Z7wjvixE8nYoiSb3WBYZ6YCCfPg36pRken0Lvyki2s48NDSFDOvaTD2rDaXZbMMcOXrXBudJ8tjXqEw%2BP2ZxwY6pgFUUxTpok%2BUj85%2BpD83aCf3DRMrJjLWB8JDbKphahWlGpX%2FbPqhjRX6DgWdBewQ%2BF5dm3KZ7kPwi4YEgRUquHscv9oN9%2Bnmr3oawcSsom2D7XT%2BZOnOgU6mGdvgBnz6xqS2i7LTTAMl6dOfvN0fMk3ZBpnrehb%2FlalDEyPa4zEMoTBmz468sNmQT%2B2YhUlA7bqo7D9NZGV2kAZGZfSNcRxHlUZRG%2Bz6&X-Amz-Signature=02fa521a71a996bcf31128263096b03b6a9514d0c7cd19c7ef93281224601301&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYDQEPZJ%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T090049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAfrDUH6B2vQZ6hay7feNn5b2x%2Byt3TwSdJ%2BQdiQ4OnmAiAmeZNelP2HaoNXMFCHKyuT3mC%2Beoqt3EzGUxjaafMCqCr%2FAwhZEAAaDDYzNzQyMzE4MzgwNSIMJP2yLvyd9R3wSwKxKtwDT85%2Bv6ESD9tg%2BM5fFI6QFOvJodY8%2B2Fhn1SLCC%2Ba4Kfak8MwNiT8kHFwIqxyvoWdJUZOEayhP6E2j49bf2lywzi6XATwFbH6Us9sbuYuCdJ3inuTfcWmO1mW7WqG6vqbOUwfINmJ8dde4%2BG3IBkVDq%2ByH6Ul5g%2Bag420pM5GeYYFVTVFpY9RBS%2FeaI4FgDJDGL1hDHXLwozvK3hOJLSyjH0%2FIlRctn3R3cdmUb9RxWgQKi71NoCvBs8fk4L2hqXgFXoGKsmcIAle4DHmNnNYco5lHi9Gv7moiRgyw%2FIZ2nhXdOX6DasOoW2b4hNyP9opTFh5QT%2FGxQTMc43Lqle7IdVZpem4UfECQDk575rJPPQEVK9gP2sljGi%2F2y20JVEADPPl2PfSRpjjbxuFBYeQ1nS2cMH7LeaTnkyQiAvmEUDL%2B8MdghQbzaHJkXyarW0aA02jJ81HOpl1nOw3igg8DhNvFbPwu%2F4Yt5PaLrFYDa3NJPSfavOYCLh1DJrlmQgv7FXaAt%2F9e0nnHZOVunFqbJ%2BaOnDx3hFYdEz8UrWUWRDP3Au0J40CZYKbQ8KqlVpTF%2FQkg%2Bvh0dhOOEMRQsq%2Bp%2BPwm6F9NzyKPAQrBGZGVA1pcgZP%2FAMmdv47CDswj9POxgY6pgHdelQ%2BJh8VVFo%2F46BXMhEooL5pXvoNJWMt59hzNsVAJUOsozYMdrBcWrIFczrZwtUVrPX8k5iPYR6%2Fjl2VovmHZ%2F9AmF0M%2FmgJoaBfbEvBvdQuq45cRLFIxEfC%2FYZbfrFh18GnvbQDOcD2qaoq7SD074poajvfsfTkUmp0%2BNhdKr6hXt81jS1nbqCAw8HmJPJkgLY2fn2HEoVDpx7gQJqK%2BYXBrxsd&X-Amz-Signature=035091f96fd78f21570108e127b20c9c236541c613878fa2a762935c27b9bf2d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

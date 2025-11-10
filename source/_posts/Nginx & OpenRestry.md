---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RAO45QYD%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T040039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIBfOFcxJDiZj%2FH5nKGElNa%2BI%2BajqzCrO2snqSEG6wv%2B5AiBszIe4cQI6vjKJTjhGMTeukf1hjSM2xPK1qwvL3u06IiqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMKTjC2iu0IgYoGFL3KtwDXw4kkjozUCnKpVfmr4WgtUZwC1DfyLPv4hp%2Fo98zG3UoOAMe8RMuEayY64NVpahinLn3XWAeYRUUxrWuV02ITwC2hGx156GkGli8twH1Ze8AHLwTPm8uQYgMwT%2BcvnmiWaTJmsuFmKgZhH92bRRcWNMCW4vHEUNJf%2B%2FC6MBBS5NgpKo%2B1yngT3QzvVruaneHSDUZ9OKcU7Da1mz0CCEacCBjxg1YoloZigi50KHWE2j8PMasZ0cWWrNAffpUCR4satSJxO5vwCeuoPJcXpiSQL0zkW043%2FcNNaElX1RPDQ69nH34To%2FAaEvoiqAJx1QVvbm4XwD0jW3Y%2BPGGu6ZRoJXi9ePpJr%2F3G9KDe3jroqsfmhbnllap3C6WUhl8S92yMnqoUKa2VW87lJ%2Fj5Ih3J3O67aumqgFa%2BE2k5yAe4jZ3Qyn9PK9LZwSoN8sP9RYP%2BLFzmacGs3Yz7Rdwgn7wnzeMuJssOg3xh07gLM8g5tpY%2FvQjApquwoOFDOPe9vRpuSttFBuCj8MmtGwZ2vMqGGKP5nn5BwxFy5ulOPhTtunh3vjKwqsaHywfIUqIpVL4vFn0N56Uh6w6z6h3YtjlAD%2F7%2Fqi%2BnvWILnMJ61ow89RZ2eYyMKGtcnQDyzUwpbvFyAY6pgHSEXSdKF0W7fn62VRoT97i3NL9Y5tIUPhXuP7ybw6ZSAhoACZCLzyBbTYrJ15J5o9kR6U6b2Z04zElh2QiHGGj%2B0N0TRb%2Bov9Rrx1C4aDV8QjvEneFx1vXkH5V%2Bqsxuskzqvo07KPZK6O2aaoDghIJzhxnCV4EcTilB3ol8Oht7tqjdtP%2BLNwOq%2B9%2FmxUlT8vACtdXxvTNLmHPaLdGi%2FsRsf%2FJl20i&X-Amz-Signature=4a469e2af0d0e3df933a797e280606225177bcf08f979ac13759ba609d177cc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

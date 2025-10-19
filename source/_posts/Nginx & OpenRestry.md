---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PRS5DBC%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJHMEUCIDivo571T7r9OSWxec%2F3gy9NImOIutpojHPXUmlhMnefAiEA0QJtHoFRFg6skauUm9K%2Bv3H9Gk6hABUOTdQjQxjKbjgqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF38Neu9FYVbPcSo8ircA9ijYB2LHKlMBMyBWfU4UCmBEpplS54cMFtNq77VTRyrKZNPiIiplz0ffT8xHhKshLqaJVP%2Fzlx5fOa%2FyihQ01WugOQ05CrVxj5b7hgnvXK5DjT6pni1%2Fg9G3kwq%2FstbwIVE52vyevr6DfZgsTZ4YgFfih7aLjnWoWtnS8%2BMvSp2Z6ohXdhkdP81y528Br9paExch6Fe47wk2VqcgqxyWCRZYeo3%2F8%2BQNZfdF5l6WcU6hZM8LtzcF5LH6haXTf8ZLDvPBdIfR74PCBNyvgZx0J5ZNLT3Stm9FVuD8eTrNHWZ3dT1mu7om3eTjC22JmY4mEl2HznGjk9afLN%2BerwbwRjNBKPj%2FF7Gg1kIcKVcrGIEKzDv0%2FmoryoE0ww57pkPIoDxNFB193DlnIz9fZNdkOYkRJSNqu836TBabGhxez0GH%2FIeAvbyyURMryhRCV4LVy2IYZPyDpZZXXLMhX9MNy%2B8e6Cb9%2FDD3fQE7F4uc%2FbAUoxlIXcIBg1nPPGhRx0u18BIvrefQ5Ck3YdO%2Fvl77raq15XRtrghRMtpxA04ohaFhr4VA73Nq%2ByNrAUbn9fNuYpjogSfMPI%2F6otc3qXaR4fHJiQ10FLnz9mgTCQm2eBXJ%2Fcuzh2kSWYmS%2FDYMPPo0McGOqUB30OE%2FMa5g8X%2Bycm4aUDhb5%2BfxxDpJn4PM841OH1JndaotoXvaMcTixAPMwx5pX0SwXoOSMTtFx688LdFFKsbCV6stEv9Exu0jpudiiDFrdVm0nxhdDlEoF5nCXJyQv8IUR3Vq3HUBw%2Bs2yD2J9i2CKZcNC5UJP9B4vNGjCQG9InwxmOgUuEUCplBYYFR%2BxLucE2hVtfdK7ZZe2TMOhz2PMbTjtoC&X-Amz-Signature=d57f76643f44802ce36d8c3c8d5176df7790e94815d5b12cb38492ed45d58eeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

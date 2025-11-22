---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664NSYQ2EI%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFcaCXVzLXdlc3QtMiJHMEUCIG5jncR42BtGlAmUKbnpMu0RfdOU4CFR0iHPSdwDMg9EAiEA39jAA6qN61j8MXyf7Hdg%2Bm%2FOGNXLpGodt2%2Fh%2BbBZEx8q%2FwMIIBAAGgw2Mzc0MjMxODM4MDUiDO6uGh7cJ7ZFuxXadircA0lU88mgZUDs7TpTJ4n3UwhyTh3%2BQElNDnuabModbGEnsZv%2Foom5OWxv52AM%2BPjolsbdvSjjLwb2SjAKAzkdChG7rHFvM3iM2bu4VppOsToqGI8Palwzej5FSu10WJb0Hb1Qwqu8Gyyv3bE94p%2FnwjmU2Lk4ek8ixyMnEevlBQCzA%2Fkm3NuQwJug3T%2FMafzKJnr8jYrgZsH2JRRYRCyWuFlNaYDO5shDSIV76Z8Nh3jHeDPTYt4Jn36tp0jYnI7NSbMOb4xc0Mk%2FrU7SDFsVQVhk4eAFSHArAMhzEpjvoVggap62tk1sMQ5OtGL6xPuovdXSoOcg15mihcuTMjDpIR6CejRdixCpA4R1IJcN7KR91BGnyGZrVXZFZA2jD1PFrM4XuDeoHivEELx%2FOuIubDXa7LZVY01AqDeJnag7w1VDggKuMVfPrda7myU03aeq7xWp0vTSP7FuL0XCWkm4nQKKM3h0KOrJ5kOrcqTMirm2pqR6cPugw6VMc7ZLkbQvnpZuSirPVWRJi0djbhkikBCkxB1xd4sWbCtQZykJlYo88evhmHhhiAO4OmEleXVrezxZ25aiet%2BDeiGlsuODBJYF9c2KnLDo7exkrKLE79EeTu0zd1FKkIKkt4dXMPLAhckGOqUBEL2EPe9d4UyhKoAlMsRorQVQNYgqtTgPZNeOAmC2IitD34aGJejJkldakkv6n1Sl5XJXwQ%2BzyXZCbcgsl2tSNSnigsw%2BlSXSTuPgzYyHNpiuJ9iVtkH%2FziF3zT%2BcH4Acc%2Fg9K2Ejan6vBDumzzQ9z0v3CNJU5O9z7Y8NeLyRZDq4p7KU5QX71l3mc7tIVSKTH59gcv1pEcdwO0OkYKaOG4gtTwDO&X-Amz-Signature=8e883700a39b618ba7364c3cecb73e72adce69425f21308d90718d383c440558&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTFGAQBQ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCID2xhC04ggTQn3XDMGCS0LYVj0o0vn2ERFZ5wEW3EeyUAiEAhE2pnG1518iJNJfbgYufta2gOvTJKVnXkxJi2P23lAwq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDHpK2239eMCCLUZAgSrcAwJLMkWCbdR9Fnabbh9Y8Bq5XCDQWpmY1BRzYRF%2BGmH%2FQRIsmpxEFR7tFoVmo0R9nwLBBpjxHNR1hEyX%2FLq5iOGuq%2BH3HN52%2BQpielqwkkFjFKGpQfNFG%2FfzuE4vli2wk%2F3IG0wPFMKjg%2B8DmNy91ZoKkd%2FZVkE%2BxByn8IgU%2Bv8KNAL78%2F6%2FImNRjVE4QMn3hJbfhVQK3pUYHV9AFCcgjLYzkP95lXuh2VdbyDjwSdMwwNmRRR4gBe87voRPdJsYp7X5%2Fv9vhEzzF1ym9c3O1kqf7FhYVNQS5oXhae%2BtHB2SbSKTSRgBPc7uAkCmxosNcB9TVN3E2sSyuTahDrrlnyHczhHbokmu43xK07e2%2F6eDZuQP3p%2FjUtf0Ea096jxWd0%2FQAuRqTuQkcd4lUjc%2BdqYI4g5RPyqcySa6HRoW9o4sbXXicdpGZcwfHLgN6IvXviS4zPDbWOpHJbEkpCPpJI9GrBXIix1RTyLczgw74aAf%2F5ohauvBTA6rwqxr2TGJk01DjMJxl6g46EhAzbhdyEeBRtyxMMH7ufv%2BNI%2Fuf1Lpr0SKyYrpwcTyu9zR2RYPgxkPrTa7snGXglnJad83pLJLXeMSy21sTwbK6b08WLzz9WSBzG%2BYGRZNm%2Bk3MOSy88YGOqUBrfrrG%2FA4rR1Nf2MP1TLjRpY2DHV1LOXdc7gVEPWjFcJku1px08M6fnS3zNAHVG3RBZc0e2iELV9zvxjbyRZUojrLTKglsscgwcMJ3Hqo%2BJR1Cjd7bTOrcRf%2FhDNEWDR7KMCsujnbpYa5ea8KL1YmR4PLA1TCtp473WMlRrcWC6QVDcbRYNtwtSbc9faF4nIWt2hU9RevFi03PD2fS9pyFhCh0E7J&X-Amz-Signature=fdb9cf3b61dc4468599ad4552d931603de99c02a80b657e892b564fed7a3effc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

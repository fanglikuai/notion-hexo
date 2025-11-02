---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RWORSES4%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T010044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIDgo91qejHIPa5Im7Zoyf7BprWEpkpiB7mQTUj4uqZ9PAiEA4rfsOBiu9ZE9V87M%2BHaturNs8NNOfHaDzTdokk91n9oq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJoPVsYEbAn%2F37RAhircA2tp%2BjndzzvpYEV4aakZvx5eVtpC7x1QGBhA9udcUdXyW8J833J0jQ2UqPzj2USl8H40z0wAqNpZwgl1sG1VhW270q9DBPIXvG7kEbmKAJTW4FOD6TJck4Vnd3lAEKmiF8sQMKmV8h5uh8zgZUYjx9vGyanxixx1MU4FiswMoApca5WCCd99D9o4Nd9sTZql79ubmBMdVZF32L%2BXxSoFV85uBOUjKbyJicMhgbIGnhvoEk6t9Vio9OLiccAYxuJVrx%2FNEIdB%2BzVopihxzJHdKV8rZIEqPntbyKVyFG2v4lcOIHCEw5jvoUPZrmpwMqNqWjcv93F1rS26nK3uYjmhwLMPtXI%2FqL6K3UdTGey8M3mbD73Zg2ROQBCNwQNm6P%2F26X4FwgMKR6obYKe6ZdmJ5wmczZiMPXaQ1aVC3uAUSVNK%2BgS0YhzWPDkDhK%2Bbjz1tcVHbBn2ToaRs30eGdZ0xqtAiYn2Fa67jSZmC2F3DWiBDEJ3SoMwQqeFLxKZZgbY0WSZWQ%2BkpGwaaOaNigmGJch%2Bwo5u%2FnXUl6%2Fz6gttD54bz6I6rOX7NGTFsQYziZJBlRxg%2BPka8h5NSMCUPg70B4wbQfFLKf%2BrQ6zXwCkYRrbdsc16EIX5k8NpkOZs%2FMOTOmsgGOqUBb3dpnUdgN7Gl7fZ58b1RJ5aCDapxJs5dBUpwRQFHN0w%2Bq%2FM5DKlNWEic2gpncz33k8THLCMjGs3rkxml%2FONu5wBxQwaPtbYtSCeduxJCJ8fWxk4f38hHbLWNrtC6q0To4bEpMoz6z5wONDdBl%2BJ9lneVTL5FkkeDk0fgY3eeMMG2cowqUYExxUgzVNvI3QkYmnPIS8H2T5NSEL2gFFe3ydy6plBx&X-Amz-Signature=acd5d05cd2a47398e400efcf63e1fe502603b826e5e77154f3cf63983f6f868d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

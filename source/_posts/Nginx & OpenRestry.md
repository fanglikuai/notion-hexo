---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LTF74DR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIHDv3Z7CapZK0%2BYKqROdOvrfztqTNGTNBIVBE11%2BXvQzAiAwwleujgFvn43bcEPiWl7gtgJ%2BONsvy5TX33Jb%2F6VUByqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcxDttIf5REqIe6MAKtwDWd2Mb7TJj%2F3osAlxZSLRDQI%2BJO4RK4H1qVqpR36rIDwMrCspX0pn%2FhmvvHtUa3CFQxibj%2FOOsotaHXR291bWA5RDncWkJp%2B4Ahm81rL2OXT2wFa8cjbUy5Zpa94PNkByv4KEj9UBE66cejIhRwla28LgM71BtLSFnKLN6JUAVByQd2AFCJntzcUv37O%2F0Zudpm4YCJ5pAtpkN0Auts81NrMS%2Bbxpek%2Bul1rwRPpm1BV7Xp8A42BI3iHm%2BWRE2XhE8h56CAh1TzEc3f64YsnVOlwyqqbzvPYSRXBSJK7mfx1%2FKQ44x8YqFI7c0ILIjwrnQXp37dMyMploPG3XKWWULR%2Fe0wvnrdLxzREdiShpwIPXpCfFhPERIz%2BL6v7KSorLE3t8baqVgIuZ4xTnIBiVp7zVuhBDiQqsrDkHmrbQDrr7nOQ8HK0PEXTUSOjJ0FCkeBPtw4kHehXCRqkON2GgFUb%2FzmKtMtd%2B%2BNRHeliuTolNwgwpifGJJEsaS6REGuAcRwBUGg6JaKgGyMIliKKzkiMvQDD4WStNEPSVWqt6jL5fPFZQiy8lzPvYN3%2B%2BMnVwDXbMiSA87y%2FrPs0j7XGOtP7EDPCaOdqoog2QSVhdDVzQeHLcNcDrtVO8JogwqpuUxwY6pgF5u8hXLeau2liy4clenWLf%2B05YDksHqz6iCW6az8tqtJYlGNssutf9RepT%2Fz4W%2BmZxzLUm26IROdD%2BH1wYSMCtcM8hXS5aSVXjYjbQLok3HXZhtwdZOsU8Y9AVlGN0ygS3zKumEOUNE%2BrvWWMV%2FKMrXM6CVKCYCmU7hkg7E7ysg720fgu66f%2FLhjJRb60VD0iWheBIWiVknJrRK0grGZwiQ2HELxlZ&X-Amz-Signature=75afa586b80aecfc310528f5d6c6d78267d81d0ce5fbbcab0d5dfa80f8b936d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663BGC6BVM%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T170103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCIE3FYaBGpQlNuOBptzO7YhelyICgOl%2BGmvLO27Rdd92AAiBM6aY8iuEW%2BEn9jYMrzzScLiGaqHtr6iIPDEs9xlAGMiqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMsev2Mt3uTHUXxoiTKtwDoA3Jm5LujxchwOjVR4WZbjn97jWPdr%2FxgLIfH6iV%2FWK%2FF5hYQW2bTpIA5J0TsXOzHBobyFz4VnHpGCIYMZ10nHQpKyJ6lGY1oqMi411VRwsThKkHfimLb8M6WNYyZGTb68BYHDOVT7iA9g0LBvbaZkziWuy4srhJ38VsV40pUcH2W4sPuulsAm5k1jgfOcXkFFU3pLc0fjdUWO1Ga9yRHWcCw1RCwa%2Fsrty%2Fh2Mmkfs8n%2BNbBnV17TdoUmznc8bBFJ82mOYqyDPiQCF8MNs2OvzIw%2BVYqS%2BAegjbQEVZ8JCRGlH8xRysRsijdfP6xMf9%2BPsSPfTPwnBEVyPSq1eyH6BIUil6odRR4DrPWWkz9sdMDr3Oq3QL0%2BL2B%2BxOtWcyZWueYA1J5xAU0nIA7V7%2FMgLbA5MvDOwbJCNJ4%2BQq%2FPHARXtLJ6iLOW2p9UMVhuD7g3CNFHxyFq3gVIoT%2BSke1g9lf2LtbC%2Fu4kfcKlDyNoboo%2B1pNZMJHzsT0o7lQt%2BHkP%2B%2Fg7pNN4fjiNNLewU0elfeYofg0%2BKPFiXjDZfHQ7MtagjUTDX0DFNmXBd7O9g7ZsVeDnskGWNidptl3y8dQ%2BlwoOmfYGv%2FE2WaocS2eO6e9aOo2fkgxOB%2BnY8wgNCDyAY6pgHxEem%2Fo0okU2bbzf86bfuclfkvzbcE3qxAIzwM9do%2FkJxerYTjFysE4gbh8m79nW4zWNDj8ZCAje5dP0TVaahthfMzTTQw6v9gqTfkWegS9hFUEg5aQpmTSDQO0kGbk16Up6n4Z%2BLAfB7%2B%2Bsk3nWY8ln9Aep%2FK2T9gA3dC2lCN35316ffVsI%2B4URtO8GJFz6vE%2B5TgHfSYnFScIGf2d%2BbugD2Vge7n&X-Amz-Signature=1e3872870d2043c2dbe994493b9177406c08e1e81691d08b37157f1cccbd0d7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

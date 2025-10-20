---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIMTCP5B%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIAUQLq%2FsI%2Byp8M%2F%2BNG2GFjSMgP4jI%2Fhq6kwLcwP9ieJIAiAAt54JIjX0P28IhQyD70PE7YDlIk4iPYjKtyueMTRSkSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwT8xFa8OjIs2K295KtwD2wz%2BWiTD5iAtK2OlCIJuZshM7J34HSZFW%2FW4R3BM59VWZC8Q1dJgf2zGF6bgzP6ao6KevSsPlPwqf6qBCiOpon2lQX47HyQNYOInjfiKX9VvY0vJZou7yXyUMYyWLsaXQNTciRgmYbCcF30om5MA7oPP%2BN0y3C12Qnx4Xi82Ex8T2FVrFi6J5FGGNvohLX0xKSbuqRER7yYHeqr0Ms04Ah9WfpTSL5%2FtWp%2F7n8Dxli0iAUdTPji10V7JkZwCNcYffcbvp1kcv6snXHjL8Mks9Up0T98HcPR2uM%2F123gPULnXVZvmkfmOVvG83eiN%2FBdwj7G4bK5%2FrzeC%2B9S2t0EcGdFFsK%2BkeOHvnJIlmzKZVQAMk6c7WBfmFUyCKniz8ykLj3K9zcDli%2F%2FrbvigM0gm%2FrBNDFR0xa7FrY1%2FJBG6TYuCIazBvg63ZpDGVWsCvF%2B6p0%2FBSGd6CH%2F0NfVdj1ZyXRgea3BK5WNGLMgrPopgwsODn8fmiC1NqpxPVGAnqfWhvwl0vVgjjpJwAY5LhIOI2zOlbSvpDOtiEC38HUA1T0uKD4dZki6rZ08esi6%2FHc%2BI3M01s8KrDBWxYdYYa2pTkgKusf3XpxEPWhAU%2F%2Br63dPnbEZVm6J5QDQJUdswhMDaxwY6pgGSFYSWYhfu4NqOelkaKCuOvIm3n%2FLuavmRhRKMwTWBaCmJ96wSoJVTEQio3JCjAiDRUr%2BaKXWGhUa34dkX5crhsbGFk4xWcBa%2FmuOOYtJTrKHJutj4f1UwwJfFmbFPTABk2mhXdvLZxduvbSIP097JyFRYrmj2vEhRh8KVUJYsmLMqV8f5O9xV5Jq7J7gPLhqmeRyFyTJS79%2F%2F3RIg1%2FhoMZBSXZac&X-Amz-Signature=9ab5fe8370a4e157e2ef8edfbc7cc10d3b4208a07a7de7d9eaf1b0fdc525498c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

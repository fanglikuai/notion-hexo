---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SEWL3IZL%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbrtZNrmX%2BvfdTPdDRvRNP%2BvCblND1r5L7R%2FE7JoFlQAiEA25HiC2DPmx8PsIB0jbFNMHyijNglVjGbtYsvTyqMSl8qiAQIlv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLDIT5qgNbMnt3FbUSrcA0MtnnL%2FzHNz3bYgaZXRgFWBe5iGV%2FVzo19kMG0HXUWYmoaK8MAuIWNnZUzNHFi4CbfnuiaCmS14ytjYopuntFlY2O0%2BrauZMO4YWlLdYA9MmtQo%2BGQrepagDGQkfMQED4p%2FB54RJcEQqUPpvynD7425MznIM4Xsuacgj59y%2B8U6tkNOo3nmwI7fktoFsR3sRvoIlknTo7KFmEOPiMRExV8Mfe5GlJRoFqyUpM0zsqpGakObj2ARb%2ByeUo6nPMkTTkYHWkl5azi8O1a3WBlEuU0IeRk%2BaoD1cPbygMvRyNTxiB4SORB8n4P%2FnEerRwhwtCV%2B8bDOm4No78Zm6OXXegXDqevKcF1PI2DN3HsezdOUixg5qHY%2Fa8iuP6JhhjHENsax5AYD0vVG8f4OD2KlORtmpOA6C1xZg3q6iSj0Alytz%2F4CLObridyh%2BPY1apdIAsP0aGHwSSLpO1uZKTef%2F2C%2F6JugjhEhTIWwzlrlBDlV2q7e4hmJCpVsEaK%2FumkTo7L46JMENeC32nUa4AoQXFQHopzu3pJwqmirC7zkIJE9xeRNn1AdmwhzJHbdi8RPg7J6OlQtnfmDdBRqd94wGHSDo9bEMEFxZf3xUmR%2FNZ4B4G9Jd1YmQkZmqa1JMJWc58gGOqUB6tTTNgwGWUsnSDgoWZf3EfSAIj1T%2BEH8VNA9n2VvSJFo%2B2aUyptADRD6n6zvWwR7mQilsUb%2Fpu4d%2FyTm8xpAffkJAJ0SGZT3rJnhwozWugttxyfx%2BLhCAt3tRHOhSbp1b9X%2BYjbuInp6PnM4kn0R0ebPATqmYds1N2hEh2jwTeW%2FdqmFYGPR0cOW6%2B5roYg82WKqi0kNHZn0QjZMQsk467laI5WA&X-Amz-Signature=bc38684f6d41c0332102daa45d6cdd1f38d35f9a28f301841f34513c72cb59df&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

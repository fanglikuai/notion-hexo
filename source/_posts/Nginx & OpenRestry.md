---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664RDPZLEC%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIE3G1OLwOOKV8TJlXiwGHovSXhl9ueKqGJLI73dlzkJ9AiEAlFBHR4goH8AVVZb%2Fx0au1RZTE7TqBvBU0EE7HSHx7bcqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAOHFmiWp%2B7PKsevSSrcA9mbMu8%2BIUveVXEAy7udUjBdEOnhqHY8OQH6qTvOX%2FoYEbDh8WF87L1P7hrCoMEcl0k88LCqx2Pi75DZy6RWUWBiWD00XGPCADbM3huPFUHDhr50b2IotrqnJqiiU4X0v9MdMTMWKmuWXjTpg%2FUvvP%2BTNJ9ftgVDFmc%2FG64i%2FQ%2B%2ByVhkhhk9qvl%2Fbc2hLIgpOtXU668RaMuEke6PffhvqcO5HCWAJbNAiMU%2BOdo8nwsyOglbpMxVcue2JL4gD5AHTMJ6sJhzKMef%2BI3j6hL13FQrlzL9LgTDTEnDDb%2F%2Fus%2F93hr9sjYjDo7%2FDty1aQkOCSLn9q94CS31Pd0St734sfYiWAuxEAhdUodxt%2BWyPiu%2FK7leuTkIRc1M20WyftI5TCuqgClg3xH6C%2BbreVb1Gg4SMca%2Bj6p6a5E9KGOB2vmNqoB6RupxALtj7PGfRL8QXDqq8Szw6X8NdtzqL2RlHPJBYM6Agx%2FNPKUI%2FXdhd8YChWGg%2BbJvmDgB9Dk4F%2FB8eVwHcbgT%2Fkxdx9STTPUIwU0mSaCF28PTLrR0JNiNa90ys61H5fEk59uyME0ooCBx1sL3Kzux4L22VK%2F2fw7Q0yMFgTUraVSSZUvaKpDEEXx9JRBtXXHt6gk%2FLncJMI%2FJz8cGOqUBm6DZkzciWF84LRpljRMXUR%2Fqa31cNMdibBy739PEOxI2wEvionnQcFoWJH8UnjihoQklkRrV9PJ3sZe6tdOQ5s2Ao3%2FYcoWnbMILhootlOfulE5xC%2FIw5%2FQDKGtDJ%2FfzFfa33C9fUaUl233TLhPruCOlwEUjYzTuUH3ilXbyHaubCbp2kTBk54m%2BM1wsrOjiD%2FyhjUe8HKM7ZgDbiMo14LlmzuU0&X-Amz-Signature=72595fb50b653abdd10c8c5b66ad2e12d3f7d47f848798465b387bb046d6c8e9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

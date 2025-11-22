---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QKMMSS3P%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJHMEUCIQCpNmgI13ZSb0c6ymxng%2F53cKWj9CgUuhvrIbFzvHtqXwIgY36DZLOEHD3ExO6CJ8HvOANbg4d6ySa%2FbIVcIrdRn1sq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA%2BL0Wdyb9WT%2BNUAIyrcA%2FtP%2FqcvgVVmnmaMLymjgD0yRcz2z98V3uEx2HSFXXi6obd6HoKR4fgV1nlC4yeYqilKEy6Ruj0JCGcReEUI8CAqSA7EmxIVppGLvESmZ5W9Gcm76sT5JAHFcQMuTYH301LBXAsTUPMxVem3ek5XXttm3psO8ghbHfJFiZ0woEYYYYS6ZaL51lS5YDEh4QtU8R7FDrG2hTx919w%2FICIQ%2FgX4qpPhova8UgoDtR2sk3EchUA%2BfXHmWr%2FOm%2FQrQZQTY7EsW5aPbcXARQz5XjPOooJNMpTkXEc9cZ76eIN%2FevKhV%2B7X1xpaV9QJoh51GSkirpfb9dpkUu3BuxMAxmT6g2%2FFuo1kvnbQioYiVz7kAOC845oGjZ8T%2BivlPEyj8%2FAECR6mlgFZ0kDKgbmCPvp0gn%2FnO7w0Vi6JpvoUYaXj6Ntge1tQWeffWA3JeflVrnXWDMVZkRxrc85O9Vayop4slpFZt%2Bh0D2LcAuXzSrW3rLlPw00BogBLcl4LUBGuovtY57BG7pqm5j9MKzSSSnIk57EyM12xQ%2BnBXZe5yET4PEMvkuD0z8%2BddN83%2FUTrYPFOQ%2FQ%2BjeEb2NpYyqRh%2F12xQlcNNFf1EJUdk6FsUEhFnPhQY%2FlUamb%2Bn9OXRVRWMOjkh8kGOqUBxd2i0VUZF4et0zKulYPPkQql7l3GcsJ%2BaClnObdhDMEekXT%2F4%2FG5i7PRE3mUibWtRShPPLgehsRR38OILykpr6vROZoUob8LlMKto4fWiR6y%2B0xNwGPCl%2F96C9S9ZDUsEmVkpst67ff02o8BhFW5%2FMZXiJOppK3fuU04griYGb3J3VvrY66J8ePL0TTdp9tJh3QCrFlNKBJ3csr6eNNGuArUmE%2F0&X-Amz-Signature=30807232b5b4b5b557f12999ed1e170f442ac3473a4d01cb4948f8d393694b57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

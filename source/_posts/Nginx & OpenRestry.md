---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XBJAR3GL%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQCYK8b0ddejPZ2YM9f4%2BdCzqL8sZUQy8tZGYzaewGECYgIgSvYojzrhtKvHW%2FMJHjY6LL53z%2FtwS1%2Be5N8nyxt0L2wq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDOlcvxg0%2BfcTCS5uoSrcA0ra7OUjr7vTpGra85b%2FPgMYb6V4Zqxx7rFhAxnsR6LrOZb%2BqsNCghgY%2Bqfg9RSdsf4cDs%2FaYSNHNOV%2FPdSEHjBJ7rY68R6xdPbtE%2Bl2BoxiTCijQW9FAD6PWKowPSicVNT2itrbPA3pPDe1e2%2FpPjQVxAE1AXi5ZybB8cWOQc%2FWz%2BMSFRWcKt7zeIbqUWn1Ik4fzbvznZHKx9YTRH6vuxnYUZVsyD%2B6hqGDuG2eRdZbsn%2FX8Wrz%2BjEkuMio8C3m4%2FdQOpsJhFKpJy0T5jVdJ3%2BRMd4CbzvB0RtUSinxSoOc%2BsE6N%2F6jHe7kVEf%2BJHu5eawR2DSpKN4%2B%2BlzU56kp3VUwibk%2F1nTrXOWlMGTsOIXiCjVfxP3Aq2upNvySQNJMNJdiZEZbqwNQhkpY34rB%2Bfxsy1U8XwsY4OXMgWEYhNdsWnAXEpG0vkkNXbpyOZwNfNdzZXCTAusgCmdsW%2FTSwzDfp3Wa6pFK7UrL3kF1lZAN5cUAC137oEi5mDKEcLis8Y0QuhPzNgnp3dsZ9eHbc4Y3FbgqtQFna3jBH7agpU3vR6f%2BHFTsDkN16yXoW5HnWPb9BeGp31zp0V02JKQZYN4QsFbcyupTBCJrdV0mTI6l29QVP%2FrbPtxH%2B%2B9dMLnUm8gGOqUB%2BH19rBrsLC1anjaNKkTOwW78gap7rS9EPcQbZQjpv3k20l%2Bm%2FjdB5OldHKz2274KoktyUGfM4MMFSN%2FZQTX5hlYXTdKhYCfC24%2BAkljp%2BbLd0GkE4PjS6vDWYp8t2KMSo3F4n8f5F9PJDXbL%2F7%2F%2FbJSFE0B%2Bm%2FcY0lVaH80uP%2FjRGNJF%2BJDcyWtEyxGEOdGcPNf7429EtZbBnvQ9yYQ%2BcoBTKM60&X-Amz-Signature=9322989e3744b6f93cc4f49ce0f2d3da7fac7a5548eefb95d5a40d7e0ecda5b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

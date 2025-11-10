---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHIKXC6K%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T160053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHesIzcOt9Bxan09VcinH2Wna6w8%2Bkt4QHjpmKmo%2FFvAIgK5bdi3xrk9jsXLCUnQp5Vf4sB7Z7QsRDHm2WJaZk%2FAQq%2FwMICRAAGgw2Mzc0MjMxODM4MDUiDK4evqEvi1mzF9XvkCrcA%2BkrWG1XvTTBMdtymcaWPVnjCHgxqPtrH1nDuxzfojZnPbYO240eTTweHv8jereJNTYP6y6K668aiQkQ7fe6pj1lXWbfGs2GimnzuaEc5Ij2QNvzbhbjiXkE8d2gIhTJ6KC2N5mYGznRGkkB935skhsHgHBuf8SwRfWNr%2F%2F3gMzMYD96ozDViigpytsJkHNjINVc9PousgC69Uj7mkyCrP9EJNSypT8yTLVuozrxDfrpa8%2FgPSnKtiga2oMJX9oT03r7E4H8nLlhCon1yTCw%2FRmB9cUVmdGgbluR5fi17ChdlY%2FIpeULJ%2FemHQewSTPW8uWV45g6T6%2BwH2aJV%2FXZDP5DPxsxISLXvzyxDog74SjE7ePOexj0CULUoXmM6AE6nGGlTD7yUamawVPTZVv9oLdOs1cfvGtbyUCLyQvSl840FSO9oHf3kv9acjyz2nsnrh9BCxM6hzHaqobvdPHuBhHbgxnxGyP7uYli1pISSm9OaCE4X0iKywUXmYOGID01cJzspLcWPdvQh6LID4bIM%2F76ilEg5nmZX3AslJ9MrnegeqWvvonZ6k5E7JivuNg9WaB3x6mEBfijif%2B9MdTqqXnKyU8XPVr05P1Bd8gHFrmbRHCd9wK4sFs01HIwMIOMyMgGOqUBJ8bhI3jFdbLYHHe7p5sSSyP894Se7lBNJA4adBnx7qEre1YwGM%2Fq0rkoOPcVamtxCbttIf9FutvJKgyUlCj2PA46nMCz1e9awq2nHQsdh0zibHzhR4kTtk7u6Mu7UGH9Z1VOGpKqk4BEkDHWRTX%2Bbu7eH8f2vnIAcDQgBGJmS%2BFvWQpqU5q0uEpAe0IOKsL1WhfniukFUe5nEpB2BZpmYMYCtMF2&X-Amz-Signature=31a6bb0d1defcdf9c83e69941a79da67e96b34a2bdf8de236404f1dccd63a31a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

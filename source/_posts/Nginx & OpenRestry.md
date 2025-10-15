---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZHZZLTGC%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T080041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAFyg%2BofLeBav%2BCIzCDtnlDLJHILj%2BnosPT%2FdTQV7BjoAiBzSD6mqmZWo4h6IjKjE1gIFZVyA1Qo%2BPECGTv8zcC%2BDCr%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIMrgxru%2Fxxq5jA1kk2KtwD50IOA0egredVCBdmzF5scZgNQ867zUR4HUbtyp1c6lbqwOEr2Qba0wDD0i6iX2Q8whGl159KYFnfExG3Ieu80wN4aKCaZcPKKZwb14vEQB7K%2B9ncF%2Bsvul59Kx3rXLssi7WkRNa6%2BpWLOv89L2ljAmFmIyOxQPdUe9jxGdAvTS41DNIpR0Q0kxoujhflwA4C970LhC5CBG6zM2Goyv6ozUvtxjgraDBnWZPU5p74rrYuX0fV9UxID9dDmfgN7%2Fy2WCYQbI4yXnr5QGnA49dDfYTo4j5hx673LVM33%2Ffejulspm3MZVYcevVMwf8CmYGF65rw7SqSkEWuQknCsjYeL%2BcNaga7gnD0brN0WUYVRQsKveWy%2FG9rbbXNYVFBH7QjKpN6kbgmCjMKZn0gmBjWT46E9l0Ei8v5dkMLOwNf8rd5gnbzK5nfkoRzZsqEg4jEEqxQkBSOTb4bkSP6ocQM31AIO5qgN%2F72p2hKsFPqvpV2jtGSnjqAx43X7zUKILL10xpsyiKf%2BTWqMhITe%2FV2r9USqbGniVABzpA0c1GLj1W6qaHxHyxYD1hSY45POp3Lu%2Bjro%2Fa2Ps9H%2FcFRqE8QqNJ15ghTwQtb%2FkIkBDtga3aFGWRszeV9x96HeSYwqIm8xwY6pgGBKJMqtkrvMpRpUEbpOXeP4k0KjsP813KUjgvR22GD%2BzL6wSiSKnWaq86THtphcjotsPMMUw5Wpr6vEkoFu0%2FpVXWIPDqMqgCgehThp8pn7eleplIoxAaLwcs83%2Fe%2FIV6o5QZAm6W9FbzF72abicNzQi4FNrTgr7dXtJ%2Fwrf8mnKbewi8pna%2FKksHfqab43vQfgHUsiT7WG5YtrDKAwYo9FZX3gdRk&X-Amz-Signature=1dcaa4a5ae762648caea5619aadcc0cb33104635c1a96a5e42bb6406607a8d40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

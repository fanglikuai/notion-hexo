---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QJT5V4X6%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIDSDX8Qx0NfGlxRU9l%2FYdnHy6LJgZN5gS%2FkgT6tnDpPeAiEAno5iVC5DJhmjodEMR4tJo%2Bo8OLqdZMJl8ijLpfi61K4qiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNP92QMvBwKAmzpVsyrcA2kRXNuzINvsERmsmwpZwW1o34vHqjnhv3J5rNbLQsKX%2FPiAGSug3ZZsIWoLcakkh%2B6nxn1SMGevZyvDEELzvvQtQMSkyeBHqk4RMAtPaK1k3dY%2FTikGcQp5wJyXwf3VoR6NUWQbuJwGMYTW2CrLGuup5%2F2YoX6LBHLb9x5rMfmCs%2F5HvURaO3TQlVEr7xUaSEPfmitPT%2FKs5WP3VE4CwQ7eH3MPYDCIcKFgzEki%2Fm2Vz98XJGkxJhH56lI9bJ3kzBe3ARNs6ipRKSbdxrMk0oOpBtx70%2FzE%2F2DrU9WqmfC92N8jVa9CUCL5j6Tpme6BeQOYvpMfZN90y08QopKbd6GgtrOPIfQ5%2BewVieeDDN3MSs7%2FgKvjbp0uu2y99OGHvZLHCxTjOMW3QIGi4zODLu%2Ftkll%2BvOrgOf2o%2FlE9ZRMpYlKRR7Yh1IbiGzs259ihPI5P3w1tnSOnJ33ww%2FxNkkFtlQBtnCgLT%2FirkSF92Qkipv2c7By1e6c7Z6Y64g85irVbYCn2ot4%2Fu6xDjVvPPEDJPNOPTlrbbHpKjRrBLv3cq8Aoti6jAOHYCPrGUP8%2F%2BJNFo5TVjDXftmVEz31YR1fJBGJUmE1Q41EbmVCROpEJQjR%2FNnK5aXnm0DG5MIXouMYGOqUB1Lu1PRXX%2FPXeCEKLV94G7SprnmQBq5lJyy3PAtkGXsaRIfMtfoZhws1LaT9upvKGkj4v5QVVofYPwPq4xPutvtM7rCyZC7TMmJQDGeXVDSrvFHWblHpsnwkjKWNrwXPMKv0YcRAL%2FFZYUTaEjkSWDhO950%2FdeYkAv3fiGVeMWj0g82LqTwtbHX7m6loEeIDVDWw4crQFKLm4%2B%2BGkryCJS%2FsX7Tve&X-Amz-Signature=caabe6e87a07685d70e7599515ce2dcd8833b03b4519a4b6957faeb7fbca9dc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

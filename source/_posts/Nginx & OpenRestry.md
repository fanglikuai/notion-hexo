---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7QPXNSQ%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJHMEUCIQCjfLtTQmEL%2FtWjENEasqR%2FKzHG5nmKpmZY3%2BAbBEP4AAIgHPCz3HwVnSw7Zhy2pK5M%2F9blES6bIQQDXqUtcs5CU4Qq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDDMay%2FVbbvIrW4PtlCrcAzXYxyp68Tw6dUqdSY8h9RR7fycswP7YvmQOeT0ZMWOwJp8GvkBHczoCiVqUwzFj5%2FReSQJ%2BpCeDVUDjMkhOQACZoJ14yAyvSfdT5VnwBl4%2BCg82Z0SKYiKY6%2BuP9kBQvbyQuDrsiWo08y%2FhBmkBzRlVi%2BjXBcoXYMRkmfY6nCCdv9i%2FDWvLQA56WDGk5%2BelU4tkDNsvRhSaFRQOjvsqcXvae1VKoXnK7bENLv7W4CD5j5hZw0V2lNjmeCNIk%2FmUOoDxiO6goWUGVDp2Zyxn5IipXPghCdw%2F5KyXUzDpRl9tJTA9Pwf8%2BQ%2FwKoFIsBM2mk5R%2BkqUNZigqHyqtUwFVS5w5Ln3UywuwebmCbExhrWtvpva2wFMN3CldmeDorWBg5SfF9CCOB6kBHbO1fWBhVqLj21qVDntc5Zdnnhu2JNNT%2Flv9n94GJZnPrtcQ7XZDbFP5CeOPw5WGgYjvhUJBA6UMSMilyZHLrlYrt78y4ws3VWiVVMbBgsJeiaFnjOi6ZK3mZkft1usoXpl0RRJPq%2BVrxGQdmLX1%2BFFlEfBRjOP1FdSx0bDVUtAbKZ0XjMeYKmvxdizZtcNEpyW9e6ThLfCK3cHMWJ260%2FLnpJ%2FGKU9T4aDsWzX3nOCO0JTMLqjhckGOqUB0Bz4mD8zYkRQE%2BrJ%2FDCcJcBR7hoYoS8JX3te8q5lF6CKZ5N03GyllIaMTNH5eBdtfcXuGJu8RXuM6Dmp7Y2sk1HLVG2FPb6fhH0%2F3x087rZYp%2BebglkbR7%2FThE2KUBauJrDO53GYTtfoO6OlR0dgkoGq4lIA4ZMVsCa5ti4euT2tgoBlVZMyWRoLFVeihdWTrz33aPi%2BaoMquqEbB22C6d9UAItK&X-Amz-Signature=a262202ceb83786ccb695abc0fdb34d97f55967d23140b6dba2fc07b6f97825a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

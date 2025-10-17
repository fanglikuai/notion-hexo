---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE6VJYQH%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T140305Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIE0T1I0FQJy6dnjZGKibgEfu1wKWDFy9FpiEdfRkmPiCAiEA%2B4SBXM5n%2F1JiPpsOfLVhJcCI%2FLp%2FgX4oexzGMRqdTo8qiAQIpv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMvJUY64NzXJI4HM1SrcA6D9LgmWmOBD4pwlWNYETjcWqXas5VEl6cr5jQ2jlQhwCvablUuPfpq8j%2Fjm225bi2YLwFjK7EiuAVFhvUPZDg43%2FAFA1z9zFPDdAtrvM5C5xxwuxkD1e9b9u6XbaDh%2B1nF44brIhnLmFEsEYp5kZys2KgLX4%2BN9RyUsJNsJf5A3iIuix3aDytf4rMzB7BJiKSSojKxYvGCrrujrFow14hmM4ASc5w8jQz%2F4hwtisp%2B3uzGyuhDM2%2BUWZX9dp0TjXHCBbU%2BYbX0asrDv7ioxZqRYQZdcvCRLLHYrtWyRudbWMDp0429T8cMxdtAasfexJG9q56xX4CXOgC6xWK6gVGwu%2FnDzJIiHsji4pDwBH6fPtHSeCw7TyEynr5Z4ypM59x2YNsKPZt2%2B8uD99mknUf7mxTNCMFdnqRWFdass%2BNgx8wegcbtpgpq8V3EHvRDS17a0Q0LvaWuwoSNujO%2BRDGqGPdECshr3hxU3rNyhUDtaE6ehmXqaB8YM43G4YaeMlcZivKzIMhgQh6s2HjrRs90oeofwxqnkw%2FpHbbCnlnCJ0QPUNt7f4y2kLMS5Yz%2BncYAQ7j%2Bodqvc3hovIa8bdvYD4me8hfP6DUDtM%2Fv24MutR1o0yh%2Fp8uegMEYwMNSByccGOqUBQiYzV%2BKeROGnRgryHIoOHlZ4S%2Fj25J5UJVtORX1sAuW1UYp4q%2F8bKY8nnpqF66OvNgRXvXJdyczo9GZ8VmS1MugVkK3NhHLau9Wa9X048koTfWt%2BNftkKQNcVEWq9wOB22xFElWlvAo3AkGla2Bq4sHPnrz9Sh%2BvwfiXGcxC8H1kpzG9HcC4Q1CVAgARmGwEiUqtZWAieV6XVGwdmoF0ywf4HnC1&X-Amz-Signature=f6392367886e9c3016ceb32bfb5adcabdfb95244e2e968b0b61e74a3399b641d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

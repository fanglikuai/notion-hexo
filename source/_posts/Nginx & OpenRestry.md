---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RISG4RN%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T080050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAAaCXVzLXdlc3QtMiJHMEUCIQDnC6oKVRwGHXfZ1kyS3kF2%2FZGG%2BMPpAEC0UECo074TpgIgMbdMbubjeUGiLCSCnotxVj%2F8OhbOG2G7cu9OxxCy3EIqiAQIuf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPAoUgOvfcwYYvFoIyrcA%2BXUSYzEPPxuVRcMWhxXX%2BPWZ4sqB2SYv7Bjh%2FwGItJjq%2FU7PjhFdLDEg8my1DCIK62MpurHG36Il2tlvLLdHH2Gyvk47UitfzoQPwRl8cHjdtmXcaa13Z7OtaVWNcfWiwaODCvNWv%2B36Li7nw2EXiUMp2D6%2FA0vcdJ8v%2BGiASLIMDGvgWk1UmF6nE1OueTP5zDVfLoN%2FkupDmVNBxe0%2B7ufWqwZd8dSFuk2Q9gGqX5mXBD%2B56CkJFO3jOdPp5o2wJaniofKLzsKDqn%2FbC1j5mqMkIO0skRC1rBV%2FWwBipi1DEYv9szPUea%2BOC%2Ftq7BFKzD6AyTLP%2FYKLfwmiHmDjtWOeipsW18dImRxrBSO%2FXJV0IehUw%2BIxNk7mHcHNZIGkKZOo3IdJ1Bj4qdJwg49JIu%2FIlJp5sS9O7D4QrSmiNqckf3f7CrlSIbqp8yJ2mXcyjZhAzChcWi1JLm3qVdNDg%2BD4u01hdEhRgkTyqstV6wHh6icZHhROhy803T0UbPnb77uXFU%2B79sYwQP0G9LsCXvI1S0fR%2Bpfyd%2BPShJvPDIkC3TeoUp1NgpYntUZqgXlbDBNjJTIVA%2FvY%2B8n50k0nrZJuMUsaxNz4NbhEYAugK%2FxZuIligoJx2Ixsix%2FMLbjgcgGOqUBdevUeGpkCfyN1tB%2BqUSgLdMRSaHovdXcHHgwYvhcZOyn%2BUdN24Pg8d7aJ8CvRLAvK6TZGwRYwZt28tmjRCJmBuVtYWOazMpt2UH6fRUel6tuPC%2BxVBfhwi8OTXHXgK9GRczFMLUGsUaBAanksTjyVxAvms85o%2BkNCjjK84Px1R8HpYyGfNHx7IHNoK%2FPXOEi0ZHI7KKuvRhad2%2BVDj%2FH7XcAtYmN&X-Amz-Signature=24b9a242d9a21055627b0cc3b002ce07ed839fefe56b3fb663085079412eccd9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

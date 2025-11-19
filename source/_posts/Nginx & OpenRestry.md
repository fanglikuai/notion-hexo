---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YSHWBTIY%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB0aCXVzLXdlc3QtMiJHMEUCIQC0O62ONbckHLjOAsv%2F2ubXCg0J6szJhYgR03Jao3m2KwIgRRIVAVKp8LJv2G71voVvixxp7Y4L6lvhfaCRtHpv6jsqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEwDjMSgFwIfdwOz5SrcAxQERd6kmmif6OTdMCLW5kU0coHV8R6NWDB2EMtTk9CzjHHfVUAsE3gcDPvufLmuzitZLKVui%2FKMnAdIVAJg0LzAPaxDX%2FCnJLahA74L4Efxhhjsji4yizfzuPD9lXiAVFLFQjh3onm%2BZAw75tYDyRHwGSFrQ%2B7ZRZCUkYGP87LEyozMZqCLxTnGyLOPgS0P%2FxRxi6Bme%2Fx8yZka7VakrWLRXBH4ESw38GPtS15BUGeoblljRYsAKTuNYeybPqH4gWC2wXso79TOzGMpLFWlOGL%2F2ikXXpsbEACdKzh5ebiXgCfL%2BfFu%2BqgTlHsjpD2TRbTpsbHo5joSkBoR%2FWsuepS0RrFGwFHskD8gVNMYUaRds2nP2Tbr0txlx8nlMj0hvBXOccGzGlaIaB1CcVe3aLVnDGp1Ct0r5s0lThlJMjuJOGgrmhRcJWOSDZw8%2FVtJdWDm5mwnRS3lkmmf7eNAaUH1C73DgdoT5qw4pM%2BpPfiWvkR9Hercl9mKniMueBM0iYSHDaB3E4vb8idjhR8lzsNKUGHZ1I2kHScZvyrIcnyiU3brbkt%2FdBM6bJ%2FHLSEvQbbci8KbSSehTjdnRyNdjE5D1csDERSWzSyPiV0jLUA1YTXsXxjDVHEN9JsvMIva%2BMgGOqUBi4To9y3%2FjV%2B305F7Hv9QZTHEK%2BoXmTjsyqtntqtYn1R8B%2BYzc9kjRNRL6enympGGDL5%2BmDwI9JwX6Rh32PZqDCxvq7CFXxrmd63FzfC4e%2BFVa0khExlb%2B%2BVkExYes%2BqOr1eTC2CsgXHWPloUbHnsm2u377M5O3jdaygYOgfEy2sdgh6scCc3ZB0bCI%2Fem6Ma6z1YAqXxl73KeEqMpyIG%2BZLtlw24&X-Amz-Signature=7eab59b9079b8fdbf765a32c35f5e6ff39742d5589df634c5d381c38d7fd36ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

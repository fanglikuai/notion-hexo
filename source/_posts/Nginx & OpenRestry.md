---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466754E2TLU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIAUGyC3HGLF8MhAcP2TxzL3J8ongq1U%2FkP%2BCYJv0wRaCAiEA3bf59nfsuqM2fx%2FHSVPndULXf2qiSUJfeaCuENfSc0Aq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDL2FgBilS7tHOzbBPSrcA1sogG6WTrdk0I462B9%2F5JebI%2F4ga1NKo2kMSbIxegBkU1bQNL4kufXLdTvo8BAM8myzRIkvzB5tGprhaPhg%2FL1NmEybbvK6lE02Lmb4TaKzagPGZi41mZdD%2FlTOhBewZ2fO8dPPaNHTwrgli%2BNWK38pa01YjeXvCVyQdwz9hTN%2BO8N1tA7CoZZJCVzJtfqBX%2FmH54JyHVn6FSqfe31gSi6Mq3q19W6OPzQV7KbERGijctm9gpDAM0Tn%2BIfNJO%2FoRuwaBQBRqaxk%2Braig%2F3SespIWnGDDH7m0Co884iHpZUUzJv3gXI4YpZZVgj5cDJGJWhj%2F26NTcKtvb%2Fh6IdkuCoYN05DJrloxurykQ65VNZ5LVxTPJ%2FyYKnQxNjaI9%2BH0cdus6aRvYzAHgki6d0gFOP7Z99Fxgr2pBhA6CV2Cn9hZ7WZDamf2l8A9OB7RFnT8ypQ3259wcn66wU6dZix%2Bu4eFD083SXfayRzAAhkKVqchHWd7sS3e2Wz5axUJVvT26I%2Fg9UGY8FCKiswcMtEx%2Fl0AswAhsdEd4vwhQyUiR8gaBAPNabzxLXXDxAZwQF9GOE2fk%2Byime9AVnrMW0pVCL3AhNdDvX1W9aNT6qs%2BLd5gWiEXV7Kf2fgutU9MPzFrMcGOqUB2J1cW4No%2Fx7u%2B%2FoDay0%2FBaCycSBiY1rLR6tIeO6OloMcCKziY70hvkZlCXIQYmCBzmxn5oIqX7mAu%2FiQzL0UQUyi4p5f6i9YFtqLRdsk6nXkBWuNEFMWj2Slv6gd57zhu6kN2MVJFi8kvmsqFU3X2%2BNdiSwmHV%2FdccdlV6iLvcbFDQmmemAyJzHqLmqw4WnekonPL6gOHYnW6oVoxZFUzpMU4%2Bh1&X-Amz-Signature=239248d1c52e86949488ceeaeba032b5023ce639f99243fec3f64c093911c652&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

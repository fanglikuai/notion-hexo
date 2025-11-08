---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWSFCUMV%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T020207Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJHMEUCIQDDC%2B1tkAVdJ%2Fgf12Sf88A0%2FGFh6VOEP%2FTBYxV10TAbWgIgK5Ebf5AabhSYK9hoRXxLemUHIrK4WDtk7ovSwLdPoDsqiAQIy%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGfaf9iWe6W6XrdUJCrcAzyg63kF%2Fv0yEN76MnwhIteRNfRxq11G2LXi0w5H7m9c%2BdIyjVZxl7hrIpV9h6dIioWSFpKeDr1nXJYjVcqdoHgujYzIYXdi9Hl1jzTOWFswF47kur1tcIXoiPSGwiMWOLH4Ti0fDIwppkZaQiEhQ6FcHh2e6cP%2FgFDgV66PIj4HgzQzeM%2FcZhgWKhIQcPxSX3cJCxj1bvHqEugeENLzxLtlfFEaeFtQewo%2BAhkgixu%2B7blX7TgbbEmiQJBQZonbL08tQ0Yt6V3GSe%2FBSGUic2zrPM76pr9ryaPHtM7BWCl2doK3zkSEvybBR3br6pvtMUDIHuQlglosvsAbuo%2B%2BAhzKxPGhlJTr7U34pdhk7DRQxtKiCFq3tOllsRwf%2BHZC8sTd9zyGBMQ7Vy5x8J4pR1Xjd%2B74KzBnlsCLqbBFbTpPXWdk679qfXjktq4w6wA%2FB%2FiJAfsWOj8%2F3ObjEFTCuD%2FmG83Bmyeeo2ENxTiM72gQ3T1VbWVIgjym9p2SYqvk7wC5MNHrozi232AIELmhUucuZ7hud0s2lEz2AZD6mnOgRskKkNpVuntQrftxyghgUmpYMCGKlLH2fLRhE%2BoQUF9igsm89CpnJVnkwLTbTBgXFrQflVDFSuBmHFn9MIi5usgGOqUBU8BwBFxKQ9c9P57kWh075JsqxySVFUEAtx5Y5f2M5l3FFkFNdw9QQzxmISgaO02%2FOwCNt88aIAppxiXzACx2psQ5RaSRLeJ8SQTeXYN5MYNqF88sjrJ6OaMtknCBE7LbUKHjrJElyiCQ0DNiX24ywTRJTgD9SMJGFw46cQWecjOShNvbVWBriAItvAH27Kfq%2BnomYjVaPgomkE6FDPz1%2Bm9z0rlL&X-Amz-Signature=375c41034bf4fed845512cb97011a00ee9e971f40cd78ed774b2caef2a962cd4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

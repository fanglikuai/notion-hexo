---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GKNT52U%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T210043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIAnQ1qmNjCIPLO%2BRKbR87YtsOqf5DA8erEHMF6e7I%2BUGAiAdyR3vwQOcaqcqVJJ2YAoP6zXEJ%2FMGEyglNh9GI1%2BgKSqIBAiW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBvo2EYAaRGhU1XeuKtwDfphjbuYloK2pkZrySGxcEgboxEv%2FTdkf7fOO444MEURVyPurShIiMQoG6YBWOm%2B8LyoK9cGZ%2BGloQ2zflB1inNreLP3y4mJLv41LDb3tH5dT%2Bh0evUsAsqwjA9hOO1PztRlSs8tkI29at4WxX633aIKbGMa88XYTr2EtlAdPzswe5KDdGXWnEV9THlurCQv37qLMuF1BkRkfKZxjBe9nPoJseRfkF7KAiW54fjCbr5duW4s5lDvAf%2BXoCPozuqBjfsDFPJJdo%2FGJURUDkoSIPRd11qK%2Fwv3ymtvpNhjyFNuQLGVPZDHOdgbuXHPZdUKv%2BgqwBQhvbPZQchbh0GPDWeOOOkozio43L4FW%2BryAhpZiSTWqR3WlDOTJ6VF1%2BFuMXi8zxG45kNXnJzzgQu9DpL4VTBvO8YppfxUlCs%2BNVbqVj270m1Y87yCwgmQvBCRWEkbw8%2FBSMW9BbC70x1rgbq790pDHH0gpC8AYYRT02dqge5F6nlP0dQKGOj3Lc9v4oOo8EPN2vq244VlgbJI74RLSGDPKKeaYG44NFFhmr7oCRyTiKrbD7i5gAIhnGrRSaxkWelRH0LImXkCdLvPBl1BVNF279W0AfiuH0TsGz%2F8iTfWsb%2FLeEXPRKFEwmPfbxgY6pgEEiFDaRcKUv8UOfs33TzBsO7cLT2X%2FQtkkoCeFhLI2JTfkvWwINMzqyfHXTbIjqH4Uco%2FPIUFH%2FmHnH2J6kDOWSGnv3G5VwEoit%2BcJIv3hZ7CFFsD5dtOkqHYYHbwPgwe9iduo7zQpijrM4N%2FQMyz5Bzmebvd5H4eGVaHuaAEBZZZDhi2HQZZB7Yq1E7ncdttUJWn6xXyeVPfZqzjSryEae3OeRSqc&X-Amz-Signature=d14f2e926855bd0b17260ddce2293f5d42a89ba727f7427f298a41767bd81eeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

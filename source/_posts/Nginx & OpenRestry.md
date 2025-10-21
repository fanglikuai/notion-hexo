---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZGXFZNEJ%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T040100Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJIMEYCIQCfVtxRp%2FrbhVodCwFSLgL%2FBn%2FvaT20%2FwLk1CNWEQV%2F4QIhAMi8FKVjpKumiPE9SFknwPrIPFlXwv0Pb3ybW5D18GyNKogECPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxD8YXlSFDJ%2BwSK94gq3AMvgVOalU4qwtKL488EoZez3X%2F%2F2d5zqMn35ndOlHcMsJFothTr%2FZRHNJQZKCN4AdboMeq8VwA6yjB1GZKfPitdnLdUlIh1G4oquvmiKuVy4h%2Bg1DOkeyJUod2FshbtLUM3Zkzdrrvob7TWnWzFrN%2FNt05HRL%2B7eg1KCiCsDL37QUeLBmwiTWNHYR4GnU7x2aDERbviKp%2FUrSfnYvLhv39DSL3IjdL7Zop%2Fe1tBY6I2aowyzNTxwz9MwRLID%2BbGGI1fHketbsXfBb0T8d7XekSzUqgVHxwVgyxsXsGmCyd0HI%2FqhlgBMeXIY4075QKx1Sv%2Fru8SalALvyEB6cmrT5JY4ybYjP54clyxRHN0%2BwfSB1%2Fkz2yx0Hsaj7LS2h4j6ZrovE4GqhPVnmP9j0Y28%2Fl0Vo1qIRtBoVU3jSTClHtj%2BPaUTF5ShfXUvLmhtQSKsHUVXqSSmMEjVCWjKBjkfYU5ixIBO9gvCsaqfyGbaCwz15Ec6rEF8PwnVpt2U9CYk4%2Bpx3FeQZ0fHzHiYTo%2FA0DkRWdTM8nPomMrsUl8P14PC%2BSsP9KPak8nDo%2BvZMUykQ6rmmiLEIFj7PlEG1OyWNqL8OgDJn22FyZIfnTsYBmJQefuAveWaqqVWdZjcTCd6tvHBjqkAV725nr1GbxiMeWxl9kZSZ23xFU6ylBFF8X244P76yieKaTS%2B46qhPNOHOFjLQZ1zkgxWWDXu3yzSlNtkLYlDQdyGMsrsy7d0c5kvIqZIZ2H1u%2BAcff4tlFYMNZD4nb5yHNMJohGdn6y3DYMHWEahelcKFjsoUn8J25l6q1UGVNaVRPklrbz%2BvkJHF0dJzLi3O8JXFUFfJpsKUAigK3WofOzwg5z&X-Amz-Signature=74177563c0665792c29b949d7db57718bac8d10163f1194daf23eec557a10e11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

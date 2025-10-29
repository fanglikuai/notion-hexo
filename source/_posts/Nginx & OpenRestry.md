---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y2R4LNKY%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T170053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECAaCXVzLXdlc3QtMiJHMEUCIAivNQUosGAaMF4jpd7ZzC21gPho3sUqObZq%2FCbY2ft9AiEA%2BDyHW%2FkXEO68CUWkoI%2B06ruzDqiGZzJ1CbwjbSe4PdcqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvJ%2FQ6s5C%2FMm0JMzircA2VyGzzY4i7sfmtXGfqxE1%2BO7A9BCfa3VJNd8fN9iD4XzzNXjN2VfjSmB42%2Br9IbwJ5weNMREUotmM%2FA4mXH4YoY0MOZjSULpid6ZVgI%2FogrJjNADqm%2Fzk5rV3%2BkKvqR7o%2BOvhEPFt2pEnGwuvPdTnwbhgOLXm7esJJya%2FyC%2FstjkVTE9184191hQYe01zuajtjy5CrFtrAHy%2B92q%2BNZIOJClUU%2FEhSVuT8Buk6DKfNg8TKeNmflR5kdrsAjlF7nbs7%2Fa9gbKoFSO%2B9R%2BD0g0MfbTeBH3QkopyLjsWh0oxLJrTjGScAbWsngiu2%2BeVy9iRpZojg1BJnjxgHEeK9EWUqJa7iwgUZEKEJG%2FrR8jgD26t7Cywat0A%2BJqhfdes4xWEG1UrLSqv%2BA6m4Eq3t1lRywzj6G6P3cmSEK85DuRiuhMz72inFcFNe%2FTnFHTjUwoqUHmJWibFVHEj8jxZ%2FgYLOte15d2lSApvjQCN2Ds1ZGPYF9TPVndwiuqozDTF00ThxXBBI7j8rk2lf8GVT8kE%2B8mx2Lr9sarMnkwsfHYPV8FoYKGjxqPlfUTrKy9nH7FMwrOxBDPTBaH%2F7myTj%2BJLL7WII7VSIQu%2Bso7didUV9stcMp7zIsb3Fjxj9yMLj2iMgGOqUB2a7uwjmnyn%2Fg%2FZIl0%2F4TpXp5vmwVTXNAiLjVxrJCiMlCn7OwdID8NITx7cBAkBCM3NnhqoPi%2BjC%2BPPvgE%2BBPBMxvXH%2Fk5x%2FRvNoInoOUVa%2Ftb%2BiW%2F6tl4zIf4dsfXbiq%2Ftz7wMC%2F6kvhQ3yi75ATQ9hYq4WRpd7gZgroSEvoWOQ2%2FRCECms3O0EyDUJK4m2EhMbeljoqsstf%2B1dQg959SId8TWUe&X-Amz-Signature=03724c0735b06b673eded555df06e4084e697e203c2c470815a0eae454761ac0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

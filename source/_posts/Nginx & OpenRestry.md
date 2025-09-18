---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TF7VKP7R%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIHKjEIQJOzru%2BQYKd%2BAkEoMWa9gqnlyUgZofdsszFBUYAiEA0noZ%2FzQScdM1qPKITCB5lOWEBnMF%2B3Rd5mbNGXfRB6IqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOFT11sRWpfZJ%2BlESrcA%2Blw%2FX0NGJMnWVjPYKVsMWl8gImSJ9X7EV90EF2WYnDtpdxDrz5tI28iAawVP4BraJYV%2BKmtM7lK%2Fc9yxbIovtebMjakkb5iFmk36CyPEsnn3j4I%2By%2Fu0zTafZPFtzoZa0RXASZu%2FvPwZdk7TjTJNCBjxY57rT46WuUh2EwQPzrirAQprjQKYpxSFzXF2fE%2FubYP4l6%2F2PG5yAOYSalZeuhsStj378IpyTX2MnHIrQKjnh%2Ba5qIdeqSNr8GRkX7R%2B9bP7v6bRL0p8BiuYQABfvHowhP1uHh3u32NNNSNNinZjW1X0ZoJiGV%2FLrMEKgll7VOznM3O%2FuzcFY7orSZeM7nCCKSC4wwROHO%2FQuVcPzVu3UQOjwcNVQaw%2FrNeFeZotzodw1HFlpSMV9Ty4WiQ7uMxOlhu6IEnfr0078EV1MZx4qflfGj9eUgiKtJhbKDKNRhCsEo41%2FDfncF5MyyxutMJewttWUagZ6jUUSkuhBCWbAjGG1QCDxy00HoBwsseDl2zAxLTMR%2FHMXDE%2F%2FWzMi8%2BWiPMWsaYqjlil8Z4jrqdpl6kMh%2Bz6FrFtTF6L0NjUsIi%2F2673HTf3LZuU%2FoNqx6HTdsutHbZyyWaxF148rp3breg62RkPbV7gdprMMbIscYGOqUBfqcGKwUlF5G0eUey58X%2FxE6pMOvdwiz7b2fe8giz4QEHfd2M4g%2FMDFyRVsfSuXcMm2QazkYmMDWqDtlCWVdLqcIEE5tAv%2Bk1MFEU8npAixJerLI4IGgKN%2B6EL6jBtWMRUF2Eh3%2F9Hy1auiFXkHv2Rqzal02QOnH2wgnd8wEkZJczJa1I7E7DD3qzD4uG%2BXDIF8cwdHm%2BWfiDR5vLR%2FnlybP7Taq%2F&X-Amz-Signature=9c3cdd4087919943c315bb08430e2ac12df42b8ffd23a800fe463ff6367c3494&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

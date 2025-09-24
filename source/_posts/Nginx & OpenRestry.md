---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EY5VQ5E%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T030037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHbenExfNHSE29yjRNVWme2%2FEcZ7zBspWVMezTq1782fAiEA%2Fzw5GTy6Lo4kFOHVSELvajPYiMIk7tC5PII19Hsh%2BIUq%2FwMIVBAAGgw2Mzc0MjMxODM4MDUiDB6UIY32gs41bULLZyrcA3oB8%2FMpK09W65LFB8K239lEGyUkV7kkXGz37xQl06PRtZ01NXOmgWXmmg1F3iXLwzdG1SZ82p2IXOIYS3aDqjCvdFhiORtaJZo%2B043whc7ovd8FxVu47PmTLb8NJUiX0k8%2Bfwqlpx9CVgdupsvLVQTmDIcwRChy%2Fe8QLacbxhRAxRdaS5TfMBiuwe1spCKSQOH1jaMpua2mvl8nknQaw9kKrffsgyl1xs%2Bj6LzABdo7QIWIGTKlzt9b%2FQKalYV0Nv1F8rg3biLH13sn9mEP4IotpEGXeXgg%2BSkhn7%2F8gO1q%2FzfDeNnKgXosCtmRylG46wtv%2Fjr9jqC346zkLfJKz25f24TRy6UUb44YLLtjnJHMQC843DSuYM%2F8kznPLbYZSM3leh%2B29%2FHLKFEtTBA7%2F%2FrHyeCt0rlbDU6UYmRSIjzXhWkYq3LDQGNqhlI%2B5gzWYXevzyxculxpv642MK8xDtonK3RZhg0JsMiFNZUdMiYWaOwY5kkEqyYm6cwmAzPZqKOO9VUne7%2BhAJUBH69o55mHTa4mubWFoqK0wkhrr%2BPKqNMrUwXI5iU4kob19IFJy0obmGQyyb7B6RZuyYPozPW%2FEN5fuQIHPZzBAWjmh7v9HviBBsuLRAipksSYMJOvzcYGOqUBN%2BU%2Bru5gp0Z9yEgoqJmXUqm6tsvWL4nCQybNbsFdncd4gkaWzF6l2XoOrw%2FVKRORS5v4We6fRKb4RcsLRxYHVdlGpBnNB57qp4xFhhhWXRsacwyxJBfSggshoKabERLPQiq1Sc2LcBzrSfTQzjbl12tzNLd1VrgYJ4TSt%2BDUSj7NAo3sIKBW3%2FLT1zFxhYqIO9Me8FXcK%2FFB042BGyZLsBqvHnLd&X-Amz-Signature=0c4c8619d961ed9d67cd4145a186d8946d921b30dfa3c144c6901192a25ea46d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

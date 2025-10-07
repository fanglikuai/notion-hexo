---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFFBJRWE%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBYaCXVzLXdlc3QtMiJGMEQCID4Wk3E3KqnS94Jj2Kuk%2BS46%2BTevDlxWb612nlvq1opOAiBD7EoNpz61vjhkU2d02LOYX8vDDQiTpLwwQpsDBY5DDiqIBAiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbejrq1rOAmlSA%2Bh4KtwD6gPH2RDgD75VqD2P3OvPgOfJsA0%2F8wqzoEbM94XMxqnEjXTfdGGZc2hoouSeTGmqTySTaF3cPklfonP4RXdPMrtNjXCNXJ8A6PX0OS8jDey9rxJb95ryWcEN6zE9q4GcwW9tSFf%2BycfHm9Lsma70JoMmb0POSd%2FJX59F6cq9lyONZaTO880KMndD%2FbKI1mOZtgtFDTgXMbGBxNNW7eyKlXBD88GSrEtz%2FDz4MmtQZu6QcsiCsj0rmkF86Xz2RlU4DH05w6QekyqTgcRMhnQdH10Ei8KA5nwKo9TjjQfAyUwUbGWOCPAnyFqpHSqPp7Y38cLiPuu18lAtEJtmARSbBWu%2Bq6tWLs%2BgPhFGiunLLtm9xOVZqQt04FLk4LMuepFI9ODBqh8FvhfekGvA4d%2Fw0mh5H7uc%2FDqJlg4o0jPF7qlyEH1UYuFHaBd%2BkaRkHX2xcC2375keqU%2FYs2fCyTHX88R1KMi5Z9FI6rysKXXFp3RSJUrtkLKLV%2Bajk%2B5pb%2F%2Fja9F37Sp3IHV1%2FLLzA4%2BPfiVNvti1aN9tM75Y3d0wVWPuSvWknbZRHX%2BFjPlNOzlDKkZ88%2Fi6JdWXF0%2BXx%2BRs1w3g7vD%2FlIaELsJe3GxzvTZVC%2B2MrY4ltd8eq3YwlI6WxwY6pgFIdvLTk76Nx6NgA%2Bq1whzOMMcGIOmtB%2FzXQdRKJ%2Fvqlzw%2BTjn1WdMNopNxSNHshjSFjUinoIOiQketo8%2BupibipSgQipMvxIKKks9mDgd%2BsSuBOsDc7F4qqfLwyrY5P1Q5NM%2BCM5HgwibueF6XSwYuawql1ytFA5iwtsPFAOkSM2sCr0k6VSw%2Bqs3wTbbs4ii%2F2ucMHd762xw799hy7xZ4B02LWgnh&X-Amz-Signature=259e7fc967ed1d1deaf3aad538fd6d76f2694e6e09df03091632473a0895d4ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

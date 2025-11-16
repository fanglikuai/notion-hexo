---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JY73VBY%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T100047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCtxX64Jp3bSSCRinITTKrj47L0zAGI%2Bh%2FmH8YR31AQ5gIgaob7m55XQklpTXIQ7mnQ2xuXNwUom1ygMbOVMfOhlgAqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDEyTsqhmJmSlLNvVyrcA0EZOrYqxCLZ2kqzJsHbF4qYhtG0MJo%2Fnc25t%2BpEm%2B8qdj7jSXrW0cQhuOcvEpWyrwSH9CXDlcBEOuu4Ld2AVkmNFab1kXAi3Wfzpk5JYL41bLVa8s%2BV7%2BSp2vppCj6ThY%2BOFrLMhZogANzNGA1De6di1OwmHzLCS%2FEX4TC03ensXozFcWQwjlAAhnG7ojpRcTFSoYwi%2BGrNHRULLYzcaH%2B%2B5Y5RECnjD0M0W2Csrc9d1JIIQpNlLnyh2afAxgHvg0SF3PE1fYIJF5nldnm52NWNA%2FyC0%2BlRQG3CFLT65IzkEp8w3mgbwzZWrKLS1ti5iL7m7iO8E%2FnTRkKM9ic0phYMzxBZ094ZO%2BxmhQe8EajeoQDzOUcJJ%2B211sPL2Sz1IepcgBYHu2BDkI8sOZH0KduD8TFOyuKixAHzDamH13d4WGg6gEmzMwamRIOhjHvrEmnP3t8FfBk90x6eBcnBQ6l4w73dicDtT%2Fnr49Y96MrYUjm7Uyo8Lq0uAZ44UG%2Be9ZIouhW9cqNE0OktxTUVrZ5aqdwSzeRKNukXA83aCnrx51Kigt5rMbwPsnhwOhrdvzpRVBhTjt2WwyGTyQU016p7ThLo8WU0IzsV79RiZlvABjhSsOcO4vZM%2BZ2MMKj%2B5cgGOqUBT2uZhBFImP6C2HfHz03hLtaHm2l2BMEY502gPTlBzhAvMBBZ5opRR%2BWRK76jrjf9GCWrHajEYsGJLeLwtsOCSbamiFnEV1LfpKNWpsI4YH1mzGvUewJWEXX07Jo6xiCMeEfDnnFRhM7ToQj9QKvhX3GY9ES30zw77sZ0JmK%2B2ZdNfJK4NBVQYuclAhr5lITYlsmbyN4kzPLUFnSejpEWsy4cQ4KU&X-Amz-Signature=4fa747efde0076f5d164332cc3c85cfa13edc3d4e0f823a70d8f070a30000824&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDBANFEY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDiyAuXe9AIZVHoMgY%2BL7KUBmeS4Z0r0c7Dt6ppC1DFmQIhAJD%2B9pzaVXglPw5PC%2FFCgMtxhoG0%2FMQP2FETT1HuYlxmKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHqkNV%2B0xaLW6ItrYq3AOJNZ3uQLGqvSrJKp7o7nsyLRQ3fS47fDr%2BV5hVM1hDrkQHKdFV0jyGJUnrPZup6%2BohsAngUnIKN8hP2PPqHHlUtdQbk9tKe39eoOc0BXj%2BINJUuhUULxciKbsPE0DEbVGOBSKhvNWXJ00Qtet2JoH4jcwXRdwvSozHC6DmOcF0pObn45%2FTrsXppavnVk09Y2TYTZcOznt%2F5OL%2B2slhsjBo3kG2vPCoE4OxIWfo%2B0HowvYEewcEE2OAEEgooVD695CHdDjWlDsh883vn1KQPdwSXpf6l8myMjawWLcI8MFC4bNXVum3Gcp3D3%2Biju5%2BgR0dDhinHVTY0kzAPMedqEiH9H26MVL7bowUIPiY0yaTq34gCI1DaVslD9XZDN%2BQCG4r%2FLLCNBxaS3P%2Bu%2B%2Fv033pnhZEaXW6keIpmIZcJriABUWXtOLtHrRgezyqUx0jqp%2FJlZQ7qYM0qXLO2VekXKsVMkvL7PXw0kVsYIcBsAPXJAn2np2MLvi49axRqddyNIKpwEH6Z%2BW%2FxaDCgQFnrWGO1F95hsuIPuV4pXvrUJV7C3wjj5fpRJlswBdLGjhVLEvdwj3XUoxKruX4nsZgIsfQGMh1kVnlDvLaiMG4ZxnWNWotRDKsL0f%2BvcjpDzDPgMPIBjqkAejCF4iD%2BwJQ%2FJsV8iAC7XKO%2Fj9cEdXAb4t3krfYN%2F%2Bz2zxWV5XrljwxKCRYHei%2BL04SaulRPeWZYukt8zSypkbVXqe%2FLPTg6iJfzRFYTqZvgYrnNP0hXnVwV6JmPvDuLuZ8UTZ7Ihy5uecVNz%2BnjbWD2LjaLH35bbabck48fHhJeHYgae4TwePnzQfguM%2Fb2jczOk6RUUQrI5YERdwPdeNCnVw2&X-Amz-Signature=cdda412fa6ee82d6dc3f8d386985cd18e20bcecadc6dfdfbf9a7f97addb17aaf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

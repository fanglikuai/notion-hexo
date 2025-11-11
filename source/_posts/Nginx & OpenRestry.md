---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XY2U72HV%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJGMEQCIBvMUX99oFGNGO1kh63oOjebS5acpnYOmCmB9zQK3Tp2AiB9WuSWCwjzqGhhNGugEOFj7S3tQj5HzUevgNjYn3JcLyr%2FAwgTEAAaDDYzNzQyMzE4MzgwNSIMjCaNayoq8zFsO7XmKtwD79PhFTZSvvtO%2B%2FuaAj7LhMhdcznJ2BsKR%2B%2Fmf5C7rlwM2KNqb7W3hbhC95wip2A8HmNcWlI6g%2BdZAiM9NHNjwl9yTxpXdFn9sDZaNY0g5PfkO2d5nVyzuMuDvDMHxEFOjXRl1zFDo7oox1c%2FSsSIORv49jBQeeRuJbH1T%2Bej%2BaCiieLLY89k4%2BMoK96KabNcuTpq%2B7I94jSQkocOa3Z3nvyNf1UspkhA44%2FAcUgld3govqKP730JVASfaT9ojewn3m35SyMKHU5Q17WtWXlYUkW%2FSWMHg8mBHcRVSVECNew1crfbSOu8WU7%2BFO2NdBijKP4SRXcCtJY5pROwX7kvmBQx8iR0KuDfzVaRkuAR0k6rBhH8xYD%2FOthqkp8O3ObvSuPVyJIXwmDb3rTqymLgSc3INzL%2FJuBfetFL%2FovDQ70yN641QW2Deh8AVvxdTgMdaAXlppsgF2%2Fx8AGgZDf59bnqSKENWtzMsuHgHdfkOtHuxDRV%2Fka1358T0UQJDCfXm7X0eEmDWvaCPpwOtRfm6rvhz13u9nMkdzclhMkfa0AQpPovYMUHEPrwf21Y8iNidX9jDjIf4NrlbH9YuGXH5waaiW2zlT3pcKw%2F9UEOP%2B51%2BLizT0ic6wErjpkwzKnKyAY6pgGDF9HFtfxjstH7tZnLYqkWF7fe9up9jgKxNu0Ov1WCxNct53Jirj03DUreryjr1KtIRGzdc2iVY5j6nPwHvSMhnPzWqXSQLeTrF8fEr64kYxuBFEprenH75Beu52H%2BXzlg2lC1JvYD7sjNNwVd%2Bie0G25AOv5xHNEswm8YU9YE8S0QSS2ZuqBj6kKFmqD%2FGCCcB74T2I5R%2FeWhQs7uqnUm3h1d5KDa&X-Amz-Signature=9e82f8fc3d417623a7c2adb75b8025cca43eca4ce4359302e8c0009e03bbabe3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

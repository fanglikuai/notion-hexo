---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TW4LCSM5%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHnNc5Y9vLHRiHAHyrUN3Oo3owgAEuuQsWfekUzu2zxMAiEA45i9CFOGIE0E%2F7IQhvJi6e5obM4TIogZ9h2ihpwKE8Qq%2FwMIcxAAGgw2Mzc0MjMxODM4MDUiDAFY%2B5MJTJMLkam2bCrcAxDvumrow51LOU8CzQ3dVQ73iOvyXJbrAGH37fWddzYy2QGW6YADMJqppXvqtKKHHvSiMHPqngHgq8TIhSEQhM5QfgWrWoElwdFbqk84xk6nN6DbQuHRC%2F5l%2FMq0uRtRCsvtJyRv5v8hg0cZkVCvEg6TegWp%2BOfUWMwUi79BG1%2BSZHMmv4rQ9d1xGLlUCNQtktXswFBWlBD480%2Fh9QmE1cWrfyqcX6%2BBVunwaMc9nFEi7yMa4PStxEL1WUCdEXjwMzpFOU6J7Z8%2BwVYjMK2sC8EaDCHyithEXEpuGMOXSm0vV5eEPJHmfN1m%2FnjGGv2AQyYwkMvZ8EvhTBypzUKXCEnwVmOSAnDWYOBKTLfygxYA%2FRzkHa21WN9D%2BRCRno8x8tT66qESVSLvpGpy0AHZjQfl3w01HTW1Jq2VwBcmOfybgEKjg6R9dlsCHx%2BnAywi%2FhKTXUOh6CSOJQ7YQmbwbo26Kc92Z8opAYQep1sVxpxaHMtHpiqCQkXy%2BJVxZtYTT%2FAeg%2B78JFEgqo1vpyFRFFbvqi88VL6SGXJRnSSUK56zYYgpD2HldgqW9rnw6WUk2MjKECc59%2FV4%2Fo5wdJytA%2FAHo9klCYCyPnGpvbng6GQQqJmgmI4lr8nwAqiCMJGu38gGOqUBSvPr4q2mqEa6md5PrOBxngE6aK3G7Cik6%2FB%2B%2ByaEkJ8nhTv1dgEf4iKrfQYSV1tfkvCs%2F%2FnSpy86ScpS1DAgvdPRhTNVHaUfWzmMzDmgyhIi8WLeIFsr6o0xRdYLTnjIg2FAo0uGUBZIU6AmsG3UU58roPoVn9a8L20b4Exs8ev0xS%2FbCmvk2K7FqumdJD%2FL2jhxD9jxk699A9qePvvQbnZEKuHL&X-Amz-Signature=736ffff3f01a46e0be89e5da9d718ad44bd94a164335953aeda1baaf9a8fc500&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

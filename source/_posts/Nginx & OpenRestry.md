---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XPX5PEK6%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEnO5LB9U83joC2%2BRZEl8qigAASakmbCZvYvrnozAN49AiBdLViB4G4iXBsXhMBPSscE3rucF3XGl6jTZED885gRqir%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMRbJlw3FytIx%2BN%2BssKtwD5cjwWrAPCsJoWhmtDo%2FNux5votYCCfA5nMyWflbZtci4HfwgUPazi%2BVuzp4d%2Bzsf5BS3e49BGh61YpvbHjoretmqhjjjP%2FUyBpIl%2Fuc%2B%2FhW275vq9eA9iy6KHSHKi%2Fswh%2FCsge5OUU74lLqtfWbwOSb7RgTmUFzXq9cjajEMYHm%2B6xBdQoypGbWsdOShqMO8%2F%2BXEbh82rgydJgD3T4cus2tnNUcaMFxfU6hJ9XSMMAFl8LQQJmFjZ2y5JeMZ2%2FFa3dUFk0HCyfyJZIf0YOr2wTVn%2BrNhLU%2FhircuaVuMWSOVkf5v%2BNdtMgLjM7No5pRSSh%2FNT1hEfBsD7PCMVMbYCuAFwReOkeeeQQrqRYlhMx3mRKnVAGWua4uNNoHHLdW%2BwL3%2FUclz8Duyr5dCxn2ZX7YjAMcLXnTLf6gYP%2Fvy%2BWj1ucCDzrwThivXh1ixwM%2FEL98QhSQlFXXbGPNTVtPn6ylG1DIzmTj5hEAnzjwRd%2B5ENrBaQF%2FWABOGZM4JLb3ZDKmHvUnBCikwxhSS9RdqkKu5dHDfyEf2Tn8ToP7gjEJpQ7PdrqTLN9ZVhRbv3SCDxF88dxi22UgvbRC24sTyevdrs2fWRbETGbuj2QyMBHVSWCsoaZDf6i4oPqkwt5CFxwY6pgFUceBT8WJQar6HCM3%2BBaYLFUVC3A0UhonSdWAk4HZmDseHsd1qVeCJ8hld6CCZh5eccSzfBrluWkBKctcQUZFFi8KWeNKxSpMILwa8nC%2BUYWMIB5loiyS1RhqKBKiJPXJwEqJpozUo0zkHTFDJMqTMKcjh%2FN%2Bni4IeU9PByz2SAN6yV5GWUOKB5A9P0wJJCMMokVNR98PMNlwElwug7rZcdEf%2B2V9U&X-Amz-Signature=e1da566a8410ba6be5b58240ba9432bb8a27616afcc13e23d000d56c03092089&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

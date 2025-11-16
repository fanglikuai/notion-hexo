---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RJWBZ7QO%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGlUh9F%2F%2F9UfsmwQQsX85df%2BscpOyX2fUsLY8J8rRIq1AiEAsz5DBgacCRL2oupMh2QJ4TtFUahlsEHwlM6LkWMBGuUqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCgE%2BWnlPtYhpKEnrSrcAwC0J%2BVL9ciNn1%2B%2FDdOKZZ5s5l0EQMz%2FusbPdul77yJPMnsclN3gdr6Iirl6yJ9RPnSwVH45pd66aP3Z87eJ9oytXaiPsoThT44IqaYryCDjKU4REABQF6oro7EepNxpddbg4RhVKJ6lBs5dzmH4ps0M7MCL9pMeQPmWHnkccczVxzVMtaRI8durCWjp3jQ0%2FLPebqWmSHQTwqszGxQ6rlmic0Xxf4xHk46YNZ7LLfqRVpuPCprFbUFvRYLp6J4cbkrvwak%2FDqyu2ac9QoCIx3Jkx2Mu8%2Bv%2BHhBBf31LsWqd5HJD7qOp1GMzeavMXKCjoSZgS%2BYm75fY%2BEfO2Awh7q4lX3RfxVHhNAttSzRsiG%2BbVrl5N4S5NTz9Ar228keqZYKnKLZh5v0PK4JoL4jTFgdLQE%2Fk33HekakwgwEU91OR6xM29YQFvJ2yKZb0Dum%2Ff6ZIDOwj1P%2B%2Fafs7CyDrAinQWCiChZJUjTe09lK0CVDeEXVXo16w5oT2lGpFXTVb85BpIZefCGwpXAPcWYQxgyr9VO%2F%2BWXilWduJpU%2Bs4JawYzZNKWmAY6Jczb5OvOurLQyxmBnFrvugBLg20jNAIugzARJqSc453earVvCTGVp%2FbVx1KP0yLWzDfEJcMJzO5MgGOqUBZf2eq3w8cLBk6uQ%2F1vr7aAixydfekop%2FBuUClMkdBP%2FCUUFLwN27yU7MCLm17mxWDGoKjEeGNHJ4sVAl%2F%2F26hxeVyzDa2%2F%2FurcA4l75OqbxzlmlqmCh2gCrQZ8yNmX2l4WeqvPZNJHiNByE1LambW9beN66TMgoKq52DB8MQwQtEV1WiTUu7U9uZ%2BZ%2FGqZgYC79Oxz1Ju4geaQj%2BrXJyaW4VWeuM&X-Amz-Signature=2e9cf6a767bc475dd77f836ae60fc408de2f6b93ca5a37649c670c8b8cf4e7e6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

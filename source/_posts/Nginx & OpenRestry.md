---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YDO4BY3I%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T000057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJHMEUCIQCTLGaSLpfjJoeTlZKzMoN3Zk0wNn3siIiBo9DT0oT8sQIgebc5OcTAx0A9wZuUSVULWih%2Bjl3v8slq369q8DnU2n8qiAQIyP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE%2F0%2Fmgg%2Fh%2F9Y5G3FircA61O60EOOIE72%2BdNH52eN%2F3WKuwdBBvTk%2FPe3hXVAf2Cplzb7y18112hfgruQ%2BrPAC0ahEPC6R%2F%2F5KVcXMIHeu%2FSpqWM7tfqOuN%2FW2NmACLKbO%2BX3LphvJsvh43JK073NN9zEFUr1tusosbzk0LJZB0Ig%2FAMYBrUpG2p2QzvALfv2h%2F3ZO7lL1CNVbbTNJpVQr3eyL2K2zhZH2qGjBYywzuSBuxPkG%2BgWO%2FMsgFhQagCEJl1RlZi%2BmLxTFtQZfGhw9AMbovlOIINcZ%2FxkrwJ0%2BxVrzxNMHW%2Bsh8bvlsnNOgG9DXHu1ZGnNnIeUN%2Bvf5I9UpHZ%2BMPQDE8%2F5rRzU%2Br1Ggc4c3Z%2BinnR3p5ovqoo%2FmTFqKXUb9Qtmeenq5BQKqQgQPfHLpsSIcNYwA4OKvugub%2BYfXSCZ3iXi30kuzsmrirSWXxkvCKFv6rHtd4NuaNpOKWPKoajKWqEPfwkEJIPcyCPqbR5ge6uqQkpEdv0I%2BfrdRJWI55Hzm3Xzuu%2FNm3jzNJGRVs%2FUXYQGlyYfz2n8YopWlbYik4liXytbmkmw7glt7VwztweZhkOeax7vhG8uwmAwyXScfOHtouBKkKOO7QQ%2FMHN%2BJ1QE6w8Ca6z7bnWJdylDweebyvWGEdMKLlm8cGOqUBhsJRQ9it71q%2B5eFYNe6s63sppyVWC8wLksCEsmvDiThs%2F6cbq%2B1IblmOYWLi3AeUaIhww7DqfL%2FJ%2FNcZiAiRCdwlVMNtaDwtcUGMgnLjdTNqGGYZyCts%2B%2B0Uw5ti18Gm6tMVUpQ55FjTJUTF%2FX5LRz2BQKcHYitAdmeMuIHrHLXCQqQziVd%2BIjU1bf3ddL9SCA4vp1brUiugz6N1AKdUl3kVxJk6&X-Amz-Signature=0c6828e9ff67a67eb905f4251c6529fbd596f1a32a5dbd7c4cc3775c7ace6c90&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

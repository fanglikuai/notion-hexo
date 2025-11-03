---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667J62YWC7%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T100042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCQ9F2uKfXvOW66LseauUTAAMgeMRyDjqFqLBpCbuhCuwIgQgIYSDho3J%2F5vpifR%2FaNAOselKxV13q%2BdhUveSkonl8q%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDCn9foejI6qidGwglCrcA39C3blyM7dUroYIHb%2B%2F0D1beU0RJNXWMMiIcj47%2FPSWtJgHZgNv5%2FZM9GS08IfwdVWEJx7mPh5nvPmZFdzHBBxB98RE0T%2BiHG5lTy86tSm8aCNRSefiosqEFO%2BGxaZ8rSRSQq8Ktrenbxk50OB40fbM7XA1vTfJv9p%2BPskdwdE%2BTNsK29UuiU7wCf563erNL2QARQr86siwaR9QsFK%2FjHqGajjRjrCMqni7TOc8ovqhmYNXTEcWHo6gj8Nt0D5eTSKqHuKt4J471WoOEKDljr4YMrov57i5ZvllDCVQ2W8WbStXjp4d6p0WhbianOvSXOrgpcltmL9E13YSHA0689YIwTdsRHG3ZXDOB9LOO7BOP4gmbKcHHlHNq%2BkMJIDP7NxbE5lqdG5YM%2FAbACrFU6J07clVii6orVyjJAgz54FQvJiXUxRKefxZxrYCEf%2FmKMAq9vkerW0YXDJfe6yzreeqrMsa1PAxsukYTi3Os4ot4u74ZsS3jLgmmLsjYkzSNt%2BzQt6al2ndqsnXS6c%2FKb8845gLvSZY9o3dtFc9YlqZCazE8gZ%2FlkDvSbAP4p2J4M2KwM3L9qm1GV%2Fkq0bPSeWiTlMnkSMAwXuWqGqs0S6H6B0eViGeKGGHViEVMNfyocgGOqUB%2BE%2By8sNQ1UloudjlQNxMsn%2B9PvnJXRq4Dh%2BmtTk5AJSrOuWvc4GlMgKBpqGfCgaQS77UJniTD9n%2BNS24BqJ83wMUD2ut08CUkKEFPJLq4XfwERDvkD98zhn6bWYe%2BTgwOpBRpJQiV9er2xfkhnOHZRYqEOSgGNL1ucPyDuqE16%2FfaBB352qivyiXXRusYhL5a0NfpsoqME18YIs%2FcwIOWiuvSrLf&X-Amz-Signature=2829b07f7cfaa920a2f744eb3935bc054ca377594e04577dd99edf402bccd9e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


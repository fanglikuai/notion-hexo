---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKDPVZWX%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJGMEQCIA2dl%2FR4fd50eexECqcYW1jU9sNNlunq7dLvA3Qoczf3AiB5gA5CPzLb67Uqd8XougSbq28kdn%2B86UFlce%2BPplPNYCqIBAjh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMqHWJ8wroxg8%2Fm3iqKtwDHoQsfs95dLQyPfaVR8%2BWTQCjM6fVdk7YP0hvEduC7iykK58xuXq81iGSQDoQnLvg8%2FartrMJgSqLWkWj9JJR423h17zquY2n7vGUz6nBFqXgIiumR1fk2S35qFsJZET%2FhxDOGr9lpcaDNuugm9u%2FchaBjLUUsgODG%2Bs0z42bRit3cFd%2BWaSLS%2B%2FRTDQGFf1f7xvXfnGM5lZAFlAbfhGCaHHCZhf6rPleLE9TqSZK7XhSUFYl3ZKtYUYviNkT9nlAHVBSLLHnCm%2FgyY34YHEjhdvQ2bSpgr869FZqhxRnjj0CRlNMaYkXjPZK2%2B6t7WZ0y54WY9WCgm9I7Fi3jLN9u6YcbJKuihIK3LijtjmETtqGTTJOg3cf9EezJerLvqwuj68CDk%2FsIvLNwtDU%2FXdrIvGbF0jmPQ8qaCSU9swCs2jMcCmVXyGqEz2o%2B6Skw9BmXXedRgoEhgnEWlxq23LumK%2BZbK6XpuuehAr2235C%2Fy8l2KUJwtBZSTdLB0BBEvXQfzEzaDFCnj7u3%2FYC8yLzN6vCL7wvbsrLGiKcJe50468mo%2Fp5uLGNlf4ogpcvvrSh%2BblFXVWdE0stupECfGAHub%2FqOXQgDJ2kYC%2BmqN2OhaRmlFAkx2av%2BE17hPkwy%2FrVxwY6pgEQUl%2F7tSwLY3EYeTkMQlHIgpIsFgVi7Qkv%2FmX1PCsuSuqUpfR2vnWV0HVdF6%2FCetgqI4pH2qGqRd%2BXR4AB1IP1ajwyRGO6GQlbPUkD1XeuIedw4lj2WPdr87gILJnsLa5NAbx%2FxSrYr9xMxVClTQqVX4%2BpGE8Fvb4090hiwHFZMMZBp3cxnfuxE%2F0BcAfZ%2BBNjYLI24OqBmcSW4I9%2FDwLhICboSLkV&X-Amz-Signature=668e79cd018d3fdb5e9bc86e4d67e624ebbe4d943f950ae3e8b05aef4beddfc8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


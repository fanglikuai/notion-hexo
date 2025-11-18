---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLE2HJ22%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T070057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH%2FtVPW5OzXspprQBLZqnXZCZm0kiGDQ2gvhARZyZ8RRAiA8Nno%2BSUs7GEsiO%2FRRUnjJZQzpzLIDvazqao27QJ4F4CqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1CCF6wfgmRQPN9lJKtwDbgg2hhMRv0YBGoKLlDDtZJgtgxyeO2uwPCmSx9Ius1Js2uAP2Lw8ttZkewIKCn3eo5MMgoBOEueVGZm1xHqMcigcYJzuHUjgfFdVCns%2Bs%2BoQAua6V1wnMOJwiSLt3hZMtAGmfRXhixktnYkG4FOQAVB4rLgaoQT3DxpLFSU2Y2C8FaVEetEsArYapLQyG4fbLPqogi13AgTWW9sH%2B1xWzB8gzkENahf%2F8JZmooM61PQIjxlvQZ88LnwJlGGoVYsx%2F6ABv5v72vHswGKxOlTYZtRiTme27VvTzCMEMrwxXLxhg9NgV0j02z2djiLD7nnFwEouLlMyTQhGEoTf1V6oGOXWlru%2Bde%2B9wH6RGbxiF88n06k1FvzEaSDSOIXYblrzmmCD2gawCI3oRHWOcC612zR%2FT4IsADVfIUhB%2F5QTZ4fj2P7ncb9bSZB0c8FWrrwN%2BCyFqAljTwjdbL69gFTpunzgaz4tXdeegO0f97tW58NsIhYtsXona30Y%2BAmf8BYsnXK2ebP%2FnP4haE3vxa2sSFiQ0O0mvaj54YsL%2BPXnLU4rifitGHnDn4b2rbgse9ljQXgGu%2FCk9sYvXpWT%2BcDWLtKz3UlJEn3cZcvO%2FfnNckkP8DUbKLD%2FACNyRxww8aHwyAY6pgHqh2qK99L9cpEZvwzO2z7xuq%2FUAjQpPoOOs60SbpyzlAlcbimo73QAV81Z8kZ2oCK7wNVgPNgIOMv3BrDH0YxEZTcUbW3LUhPUqjQqqdsnv6SKjSix8y8hxxZYLWWOlUlI%2BfJPjl2chJ8pY%2BAo4CIx%2BjZeva88oycd7Q%2FiEhA23uKgh3Zdceae658Zzqb%2FvPyssjh9zVt54u5kY51OvOFeoO%2BMwbfC&X-Amz-Signature=35e9539db8aa46dedf214954ce4d13a348c57ab4ff8bf2978351948aa6ff5224&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


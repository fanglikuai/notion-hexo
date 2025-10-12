---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZFMQC7%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIE%2BxShgdyLC5mWWqB7Dvi0Li2GP%2BskNjWNOFKaBcrEOZAiAJqcv%2BBmrxkRojLzpruqXaln0ilJt8kL8t7%2FwMhmZm5yr%2FAwgfEAAaDDYzNzQyMzE4MzgwNSIMt7Y7vpBBxWN2yAvHKtwDfYnroJvP4HSTtgEH9w7ifcndWfw4ROSgFFDWN%2B%2FK1VMWlOnM9SGHEzhwIjWDWn172lvUmw8Kj72H9WY1W5a6xqrFh1qOwRf9ks%2B982JHE5d28f4%2BfcRu8%2BC2ZLymG8fCIJsqYvmlxmzsHvWotoK7FrXgl6Y33%2FbH56rkn6Cf%2B2pbKTQfXd8rszSLB9dy%2BAZt0wSI6oEqDKF5dapa6rOMBJqCiq%2BuWvYXB20TIoNMQYEX0cnNJ8tEbD17os6QVoDYHcDE%2BQyCBss3eDbzXqtbkA1Y1SxyZZu8NI4GN8dfDCHNxo99yyhoE0LGut1jv6%2F0FAFt1OVpLYu%2FGSw8%2FzeBq36SzkIQHm130%2FcvoqeRy4ZKdFqEazs6pFM6X1FHdXZxyG0OOOIp9AazE0FN0kE%2Bzzq%2FRv60QmUooZNF9topqP4p2Ashc%2FNCeHg4k%2FvomKXutyBT4gxsXhoX%2BBw7XgFAZSQfFXCgFB6CaVB7yGw7PTewDLIvyFDBgxxK0wYmQVbL4JqJ2EVxx01gUEOoDTw0gmUdwI2Y0lnQ5jy7g2kyOPUKE5eOz%2Fk6w8lmfxbSLCqCF2VLkcezzo5c5MB7j7xgrtX1vffFpE%2Fv%2FmzvTdPeDYR8m2HwiMZXtEJYK8kw1KarxwY6pgFSTMWUt5VP3oGX2%2Bu0R%2FWogtpxbmRANK%2FNdtYbP7nsZ1yRAi0E8t7ZUYTg20CAMelXxwN%2BHX3P%2FPNzw5%2BM2OXv2opl00JKbvM7hpcNZBFqi0UBSpHb%2Bns7pDAVFGo%2Bd%2FWBRMAwDLrNxZMAU6rGgUcI80h2DAjjk2R9YQxFyVdvyW1deBbrNd8Vmo73tIasN%2FdagMTzVh6X0OnpEWNu8fstJUHdpJcA&X-Amz-Signature=e6b5433632537484f2cc468dd240089a4ed43c07b78d44003b10ff043e5feb30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


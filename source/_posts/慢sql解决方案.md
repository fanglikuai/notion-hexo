---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IO3PSPG%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHAY2MvkgpqjlWw3gZy7%2FUqo%2BwixfCmqoRMFwhq2IS6XAiAnI%2BNG1WjJ%2F1htaQeQxa%2Bo5Ub41ZotCuDCNHqnIFZ7vSqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXy4cHO7WOzUMjWbjKtwDD6ZBToq12etaQvj09tY0QobtzTvXfLRxmNnsdy6X7Ur2m2zaL%2FYdSH3ZGgEQxdLVj1k4UjjbpT42Wj%2B4Csx5QJ8GFDb1z18KDY9rr61fvOupyvZtQAjhCcMe5sSCTxrqouIsqEN7LUVuk2c4gfVtkmQ7Morcmbr1zyyJqDNAXwg8voQWziYCUe9EUt9EHDhGqr8RQGNlJDIzj9ad%2F85IKvyxmx2448qtkDkDjc2TYWT3T7KIZ6a2fUnrrWP0g0i%2FgNaCFpvQPtf9yRvA2IC6Q9WccH4Bf%2BG6NUoIp2vhFWIoBMtM48l%2FfvQwTT01WBVEErOyRogyHkT6L1%2BDNMbP4L9Bl%2FnsaW04v%2Be4tOI0LdWiBcnSFIgcJ2UUKq041mOlsUqQSBXRaw11yI4mC63I%2BkqGmQynk%2B49KwCQHAqqaev5JMRikhGxhsiutNgaLqzyQwYkyK%2BenxSGXgc8YiWSSaBzAXMdIDbS2t91%2BZpSPnSz%2BHJtTz4jIAW7AWuasn387F0HxPM2R3JTr17OQI3EhNeq0yhZOay%2BX2SpQ8rftMRJ1cIQK%2F%2B8FrZqFb5qcfOxp9Ga8WSS%2F0GcZml68RJu4ivDlapKjIN2yiFF49F2gB2J37wp4H4Mi93Ioa0wvd%2FnyAY6pgFcp%2BFeZxKLDEb5dQvqxg3htkwwT9BPR4ye5FM%2FyA2XIf0pkWc9TVpIXXe%2Bs%2Fje5oJvMNXnQz0Y6SJE%2FzB%2BaaHBv7WuvJdqxzwnz3pAPwK%2FG1Uas%2FrJZ9MpG7x1UTBD16VLnm%2FyaLr%2B%2BvBwraVeWkYu30wCsGP37w3Yw7w256pfUtP%2Fre1KF5QnMN9IWd9DgBdsRSE9NNww1cYHeb9jcBB8IaFdOrex&X-Amz-Signature=d2c00f358927ac6a9140899699269a1e5f0e7b43be3f88581d77da427d78f5dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


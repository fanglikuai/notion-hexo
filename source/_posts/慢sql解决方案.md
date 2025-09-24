---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667LU33QRB%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF%2BlwJ%2BZyHb7Z4n%2BB1hjMPqJQIEm75Vl0ouXGC7MUbc5AiAOWyqXmw0RkBSpkrSKl0OvL1oT3XPiLhiynir3DJXLKCr%2FAwhiEAAaDDYzNzQyMzE4MzgwNSIMjWZF7jMFuImdb8oMKtwDd24qK94ZuzVM6ilZ3l5j%2FhANTK3y9m1lF9dZFgS3kqx4ZBIG9%2FM4VFmdMG%2BrTRNf%2F1iZTZT74KF8gHht4dZgEfTKBWiQaMCI2dqILpRN5qq9WXgKLgfiZ453hkCypiSBsCyiAbaNDrZjmvaxm7FMyo0WN%2Bu9gWtwgFdMG%2BmqSCG%2F1sPYtM5HmvE2JZJDforLps9A%2BUsZ2I8TKSRwgAYUK0nTBJLTUDzPBnNhe7ESWPxfrsCNif1%2FhY4C6i4mWnIbjQBpeOqt1aquNDJFCsJT0grV%2B6uzLzjxKa4Pm4owlN5nkHJkPMFj%2FkAUhrkSzdnl77SjdtTGt4DcFHKCrKhcq6U9r5VRImzvzRwD7L2nZppqzj%2F9l4fUBnX87GSC2UrIxn8s435ae%2F%2BA5zrDnjtOn2mxNvnD02KMHK9cowfKwV9Bs9VuTwyrIeL6eGoaZijUK41%2F4J%2FG9iRzThK5lrwwzVIzh4RWMlEx1CYoCQtIlLx7I6PgN4HME%2BdRLM2xUFj9uAKJSFRpK5nlFtvkptpic%2FSulQzktJLTGco0I1PSrRJMa4xvUIgdi66FJYhkhKqf%2BzYQQb0gukOKPf2cORUyQaSg819PKukgfrB0QI8Usbo29azuq65DO89tnEUwgr3QxgY6pgHITb1Ojq%2FiIKW%2FrSOUAh9aV4KqmDu2Tjiu7mJbRtKmK%2BpzPAkpOjE7Z0OXhQv36Yc%2BiIBD8L23a7FiI3NBZLfORV98IRWizSkQVrdStUtzQyDTS1i84m8iWRj1MErTr4tT2vK8t5O5GfUJkqZ7k5K9oxEVUOHZgOVq%2FRdyV%2Fc7HvCZTlzG4YTLuyWO0Knn7wXKUnXiBskulYTfDGd6TnM%2FUW00DapD&X-Amz-Signature=ed3a9404e426e427652285132c57b26ce1f2a2f7ce3a474ecbaa7bf2daf05e6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UL5SHCLY%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T130104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCja4eIq3nnb%2Fe5Yowda0oHWqVU3g8XyLh9vusVjGj5qAIgPBrvM34BpoxYIRw3Qt%2BPqKEfstqkOPV8yJF34LECqtoq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDNbmttUBS7OkLyzYCSrcA7xtwzxbJuiGwj8OkEzYGZ8myhUaFYIYw6YR34EzU2uPinIJoetMXH%2BE31B%2Fa8qaUK%2FjyT%2FFUel2YVC8LsyoUpW1tqipLSmLE9MzCXjljYBB8uhLjGZJzaGTUcuWMmckiBEy67zrox8vCvZQ4OkGMgrDSQZ7nGGY5nwp4Q9ZoKAkIUfVkjUU4pkhcKmFEj%2BqmV6RFpYzJqclrewIRn3a5ZnfoCIw%2FMypdRtYj8Nt3cS9%2B%2FlrczkHaQa60yzhew8pCqn9782hqxdQyKmpXFqffPQYnN%2F8jgeOG2d9aUV6%2FwPJXPkm522FgFZJxlo2bZBGrdl12AdTp9wlWBljeZTiKxa3A%2Ft6GZh2M9zOCBLXNy1Pt6jDpLveptpA2tBxeNZyMYMXPOwgCfFmQXJZkbsAkbWtz0406BNHMQYl9b1SY2FsXvNbbstevlkMLhVXawhG2OGo%2FCS2Ofz5AcSt1m%2FVkT31nHm%2FKLLPLUYWcjc3xi7Pkq3%2B%2BCPd0HVn4VE2Sojm4AwBA9EDsdmT%2BokOKOrk0loLWwfQpTV44JpY7jhBuBMCruDwZq8%2BXJt0Nx%2Bby9vrYMga25eebC0m%2FDeKysUV31V7kdYMlaywFnpGCmM48dg%2B3ZWyu8XEjvttt7WtMP7m7ccGOqUBzAFVBId6dQ2sb%2F9zwvNYl0ZYUjAEeuNfQ0Qb%2BRGHPmoCgwEjSxeqV9MN2vr30ZGUzBsdnsr4zmxMkNOZHB2rYh%2B1eEaT9i85l68O9UsaW%2BzBt55in8AdJpTzuR36h4yo2ueWJARZcDisb6FegmFDCR6U4L5iRBrcoSUlEKZNBHSSCx0k7uL5a%2BHsAGjGFi1r3jqY4BdsJ3E04Y%2FcM2mdDpwrLQfV&X-Amz-Signature=a6a9f9f7fe170b8eca92ced17f1fb4bf5c71009a99f24d079bb25ae346d985e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


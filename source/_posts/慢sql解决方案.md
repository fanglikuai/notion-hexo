---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZJFYMQF%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T230047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJIMEYCIQCdKFs%2F2bqvGQl9j%2BBDD8q3wtsEXeb2SPMBR45xvkLgeQIhAIdhV95jnUIO73zZ1urCOn4ICpwWCEkb8f1NoUoz3%2FcCKogECJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwK8LdUBQQi1ncYUVkq3ANWcTUe5sUIwk322n9oM1HGzaL%2FuXl1O0UJsFMM%2Fs10pWGg9D50%2F1OrY6EiFugKVVACa5pxZxYnLoHq0ye5VIwcfDkq%2FY1J9H67Ptw5N%2BNWVLdEnq4ZnO7o0I9YuqURPKNYIrNQ5k1VmW0Ju9Mw4nO7oMZ7%2FZzCnxu4MetmudOkPmyOZnWujINOj6jzkl5XpsDrc2KtYoyq%2BGKc%2B8hLFABK1gZAVl%2Bi0PzOTZBAKatlopcS%2B72g%2BpMFQ6NuBktp3gcovXXz23KMijjfZj8hF%2Ffh7Z2FwwGC3F3OdMJghyicoS2DASKr7ozy%2BVCAIXd%2Fz3v3WtIpukw3U7hGMzT2%2FBH%2BKf54qNoejrMpxDB75Wyn0xJDD%2BSRK7isw62qt3lu0PBMJBYJlyRg%2B9wbIs6X%2BCFZtA%2FtyfTnHcoElebFedezTVeBjZu0HLIjbSkbViKoPqyyGi4kf%2F0VnJ8iBsMlNLs056DShGTZ876BFtMfwSsRpXc1TipGLSa9VXmPNnIwFCIgmtIaNCxpwc5h1P0qh1Lanjwd2n1ihuj8hZsOd9%2FSTDRyaz9inTgC3oeYIz4BsjjnKtL0LPnB0PwI2agtXl7Mg9qjZ22elg%2FmVuldG0GbvrQcOWi4mDHlap8KIzDWstzGBjqkAW6qKCw1d6dHm7udEYB4LoRuDKhX4rveauWNhGviqUF0hGS6wr%2FC6kS52LPSs2s9d7rdQ9Y0yApGEtmEDCZQgy9QQ1yAo%2BdfAuT4tkF9jIRQDEPqD6lhdUY1PbZdNqWC4Iped%2BEvuK4wvfDQw2mGBka64JqMORIizknmINwslnbw%2FGAq7F5HYfVxLAMNBVtjfdkD0%2FVsGObEdUbtqTKO73M8uz3c&X-Amz-Signature=1cc39118c52313217e326fc79e31ddc20591f64d0818d2c6ddee0c3cef0ed3ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


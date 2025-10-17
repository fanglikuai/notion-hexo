---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVE3HNC3%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJIMEYCIQDCjMaw5ZJ5VhR%2FMvNhrIcttAs%2BGBpbNxyJu0%2BklS8XxgIhAKrSSqxSgxMumK6T4BtFJr6CxqkoU%2FpYTu6e%2BeegU%2F9rKogECK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxywfZ8hpVNhDFjnhwq3AMBcHPXxAaMPmLrUVqg8wbit4%2F5e7A%2BrEf2wMmD9CmnmpWQvjoZcj%2BzJyE%2FNG50vZWYrSY%2BRsb3wf9wNV8vWuPVTCcyjuQtnHhocKIopKkuWviDBxvUyB%2B3jYbmWfh5tw3spBQeuOV7H6uvzx5dvPN3WsAuq7pLCXqJEGJ0KHAXA5WpzeWd%2FuJkSL%2FWDZ5Amdfz3ClNgs98YXRfZRAdFkcWTDdw%2B7CWDf%2F7UxVNJ3J0UdW%2FHK6yMqyTohHNNtsWVEfEy0SkkNiRI6YmHbEurRkFcb2woVTGwShY5Rj3zXJJCCm6pBE8BIibzwfi3xECSrHbjxpK9V93AA9qMBlsEE2l1nWxcfDNbv5a1hezG7riabSAWTaxpnJB%2F2SoWtP%2FZlhdmCE90t0%2F8Wj2cr3CiPkyeSUV1PsZfNU1QLi9UVtXt4UOI%2BEfS5DffPSX9KNLneAM93OYa0s65sEzCvG8KHWA2zGHHQWyJXXKkAJRMnDuBn6mkliaS4%2B%2BPN2nzU99imYHzmibTAzofyM1Q2Qzk1ay0ZCbOl6cGMKWN%2FZ47XnSFOyGEswAzfCbN0l6MW9Y4My%2F3qZozzTslrFcmHtGTWKARkjzGcQL5zgjJYseSSCzOflW2IMoSPJJFpHVBzDl18rHBjqkAemafd4vNamiPhbDUVPEbIRGfASuTL98RYWqkUaVtCWJ%2F9UdVBt%2FBHkGZbvX6NdNt1WqgoawE8LBT45dw9r5WuUFI%2F1fwXJsb0Y%2FDOafhe1sIDkvHf%2BN6ZI7vrfeqKTzuFeac3Qn79WS51lpgBNYS4miVklGko8DrnF2mdDQzjJRJSH9vvIuJIXIp1fiqCbP%2BZozXvMM3WNbuu%2BPcUTZTD9QrVAZ&X-Amz-Signature=a8077f1112e9db0710da386975161d012f4d473f52d9d6ba21746e1df6e0f03b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


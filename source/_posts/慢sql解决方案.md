---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WEK7UCFM%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHsaCXVzLXdlc3QtMiJHMEUCIQD8JaTz%2Fj1soRzifOoIHTWQkB6Et6B4zW%2By8iEXOkRxKAIgRVs0PI1FuEcgE88iaJ13HP%2Fv7gG9sAeTrY1lvN%2BeX7gqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDP26T1eDy%2FLsbNl3ICrcA8bSFuZC6MYe8f9Nwvj792Y%2BhPS%2FoO3O5g%2BP%2BS2%2FuZfubXPjWDXgGn%2F79kdzSNHHjGjP7HEHbgo4o%2BI0WTCxZdsmVCe7g0XFbYHYWeosRMGpyVoLvfm%2BGeP%2BP4cNrOGXoOoGqr%2FrNgfjzW6BmL9hx0edN1ubrhFSFmq1E%2F0lt3ReEupVFEllbleVyWKhi7tBny%2BCJSIfUG%2F6U0vnl9a94qyY8aT5z%2FZHeuNoDM1vH8Y2HPwFNH29FPkBeLiTsAdX6hTvEgc%2FU3RupOnWJOXLqpYOaN8xHnvZ34X8a8h5c8ZNojJVbPTf57L4DXfNDlU5mZTBBW5gUUPDuqAQr6RbqoI4X1Bq9mVF58Ek9qLgXfuOWv4AhQbEy7n6g7HCKNk78%2B%2BL5i%2B9QPr2zylz666XtRdTDFWhH6Bi%2F6CHcqP7L3afVLF7AyzCFJ2vuuEZIwBH%2BSJguVNuFLC3EwMrmd6BSRcExoGED%2BwzJpmrR%2FrqphGS8NukVSV%2BD1uQp3jOCK3KCSXfhPMB77KEDkUM5vWs8PQrb9fpnrUef%2BQAO%2FRj1QgxW3OA3JlLw%2Bw3voZp01EE%2FScqgcMFoPO33DsKgDLEC2LoujZ1Yfz%2F7QG160KuPSA%2B35s8IsU7L1jsRZsqMOn2u8YGOqUBpShpbIwnWJkqHrJFKNT%2FOrcN4S8l14FH3ztPFRpWDz9K%2BGsSDh0odpiOrrUhbTLjeijklLyNJy9pW8kCmZeMiTqkn6OgeWqU9YvesrZFaJP%2BTNNK%2FWykn42TnFEL5psfZ2WeQ0%2FaHQ79xSRwYr5%2Ftf8HFHdq%2BmOCzuTnmTD8vh8wPVxtPjNhb4FUSxvjKy2jmVZJLggUofEl0ZDlbVEtgJU7c3qC&X-Amz-Signature=304ae0fa44fc0b98410bb3bd99a8587774bdabab4b0c3b4c1a60f4d696ca290a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


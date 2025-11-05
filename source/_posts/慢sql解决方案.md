---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SQVM57FM%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T090042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAbgk%2FIhtlkA47dRDkEkp18EUw9ogsvao4pdjcljMH9DAiEApBRG%2B%2FULP0KU1Ja%2B%2BTO92NYQF0uxVso6t4%2FPl1xb5MYqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF45NhqdlUwUA3W04yrcA%2FEPovxBCn4y07Z0UIz6ivCOsbDuOuFjexiCU5pKywi7uKStfOa2MyUbQlfs0UErMweu0YCdZNYpUODk1XAffclPmd9iZ3Z%2FPQQZ0IOPgE11dW9PTGoxLyHZWvk48c1lwdj1e7DwhNGdI0%2B64L4ZU%2BguCTgKFfQNQWcEZBh8mDAoMmf66hfgV8Nxjis5ry8Lxzcv8Fcb%2Bg7S7ndcDwDeJ2qS0W8k6XkkNF2fWUcWJYsbGrJaEj9tK7w8Ci2e7yamKzVZDNHy06cfc3CWHD0Bqhx8NYtVhp48zg1zvOjPqNQA%2Bk9fpQwzdKKDNohNZWdemEFq8czVu8y0cqjYS%2BvJHoPLlT91S0%2BJu5GvdaaowHSaukfEXhMEqya4RrmUf%2FakCs%2BPM34hlXABhy5Ab9VCNisp1S7kb4HpwzzXC%2BNO%2B930ESyvKh7UVFyPTOYB5L7RrGExWtticeR0fhMiL1a4bO3pnvn4m3X%2FdYOZYwXnoM7iZoaX8eP4KFZIyzE63d4kG0vQXcG5KFnK3edrC6bHGm7DP35kZtB2pmATZcYSfkoa7pEfsWiTjo1UJZUcRp2F5r%2BTJITvKSjUnkg5h8QQoxhJmzUU3J97hDQWOou6%2FSwQd%2Ff9ec7w%2FWJn%2BawjMIeGrMgGOqUBCADOKsCn2OUvspJnP48%2BV%2BnCnHEBrSzymQ%2BCy4ufrCkJoHoJ4T2T5d%2BjG%2FUGzIpMGcmNnSd1u5Sotp0y5EwTEYV0a8h4Hx7iDRvBy1r2PLxYpNn9qxrQa%2BKcGpLJMefwjD72SQwsuwwL0SV4hSlT8VLsDFoXaYqLVfuAUVWKlZ%2FSu5lkBx0CYeNaOc%2FBFj2BPb7ynIdHvV4paElNYiVPyWa4MNFy&X-Amz-Signature=b5f87b787745217a80d2ca7b9a22d9e71c14fce90673780fb70cbdee1add125e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


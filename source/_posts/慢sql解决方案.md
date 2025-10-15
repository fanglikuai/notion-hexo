---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665764J6DM%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiapl4dVjIgfu7dvqJ27HfsFVOIQjVmdmyxiE2JiBclwIhALWek8%2BYsQtebeS1OXmOmboa8EqQsWFXtPRt0cLnlYOxKv8DCH8QABoMNjM3NDIzMTgzODA1Igyxy2bXKPMrt8Q5N6Yq3APP4tBgEgk2wHAO75GhsVBT9F8YG%2B82fmwXEAYTi%2BWiOMH18DWff9J7EmCJCBAD2gzNR0GzZsAAk%2BkftInuO8tAlleTQRZvmGXxh93QKs6kevJFGFDrKCALsvnkg%2BNN6wQk%2B%2FJE382BTBrYdAEm%2F0uRK3njFA%2F5Qlru4ig2qeZ9hk544TOuCgKMxfj%2FhE5KEbvgMVhHUgmCv7RDkRKm8nMtbpiCh9GHEN1A6nXxen6UmIkGhGtMbXL3hu%2BCZNbQU0GYjL%2F9j0DOQtHtNKry7PO15TH%2BRdeD2ISR6R%2F%2FkSiJvXXdfp5tB%2FaL6eCztp9XkglJS7GDJXKQeFL0LBMIrcgku7BQcWzET4%2BYTRgX0HLKQdvGgqROVOY6YszjwYXkKL7wmvc%2BKZCfa5pAkegslPz8zeYoxddiHEbmfXF5LIprhNXTvW5Ybf%2F6%2FePcPNgnHNpnGcauA54nqrsx17zUMm%2FihJ7yD9sOdKDWH3GO9Z9dAqQzSm8%2BfQFV0x9XLemhfJVo70IlmpmFp8U1qNk6WglVi%2FcFZL09JGT9daOlOJcOhu%2FsWCvQjjK37qYUDsZ9HtrX4yYiaJMGG3ckRN3ygE5nQJCWXBQgNQ818DRnyTZWNcUvGs5lyJrCeEESkTCetMDHBjqkAd2ohZf3YHI5AhwILAvC7vmE7PbYgTlhMWW9T%2BDUIkkw8GP6bKqeJ%2FJT9G7KFXYMgfrom9OhwSa1KQoIzQBkTJUKHE4NyObdlVepauxky6ey8eOlhfl7H96Ytt9zOBfvQAaa2quEeEZBWQPflkGcnzkLe5nGHdHyRWqDwOKE4cgCziS%2FcDl7ZWbGxoAUC9UjGJJgYJ9kgUEB6ARrq%2BaWwsDOcYcl&X-Amz-Signature=3749dcc89a44dac1cd5e3b33db7828f45b5bb45b32e2e014172fd7cb53e225f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


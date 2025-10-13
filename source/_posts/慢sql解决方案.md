---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662DRPQECC%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDMA%2FC17zXB9zsFEcmPIij8eiZSiU49PCZu36v6vvNCdAiAc%2BGbETqIHJcQZ8MqNSAVwu2jHzjmq%2BK%2Bc9MDntvfcFSr%2FAwhNEAAaDDYzNzQyMzE4MzgwNSIMQTHzZv1jVM4MYX3HKtwDf2NR7fXk4sngC96224Qwa6ZLCBfUKK02KDlkZi7qHnvRdXZWa%2BLs7P40JJ7lbj8yNWf%2F0RoZilcFkh13a6Tpp1xcxM%2Biv52%2BHCFkCakkLzOuE8GajzKAYyMaDO4sgEWosd%2Feg0YBa6B15AqQ6xnlqpnqawAeYe7x5R5Z%2BMfDToegOR5QNTICQwOIiOxcBMWwI%2FIhZ286gQmWrWqZrn%2Biv82PwExvVTUnwKMAVS4VtxmVIgUpoTO6UGwyYl7bjFFFQWHsS0MPI3hedxyeMPugTyxvMf7wDDfX%2FeYo%2FEI3P5m8heu3w5mQrq32RbmHDy1yNmyKZOoHMpENnPzKB7f8O2u0FKwHquQ5dGJBr%2BYNYQmMAx%2F08gewMchUkyw765A8j8%2Fh%2Bc1Qs5sZ0ODATb61cw%2BqNaMSR%2FzVXkmnSSmK01pFTz%2Fv2nBm5ZPBN1yu7FwLCOUZtvUb6OtT5qfmb8noRBqfXIKRH6brcgfvISWdFrkvlleIgPfKo6WFSD8ieEfuWoTJOBNiDj89VB0Xf0QSMbeY8C2ZDx%2B%2BBHBGLQtUzyXvMjCllFWef7wdXnULbXKugQ7eaibjbiR6xOnu8WO%2Fcednb4b%2F86zuKjGpAAzYTKOcOUHmiTf8HASV2G8w2rO1xwY6pgE1E6a3VjTE9fOfZ8zJlD6cUUcw%2FVdH6u2vbnS2iWEC6yRwrn9ayaPJ0AP3akgEHRM3akIGmSuKSga%2FA5%2B8ZGpOilnHMc2yQP%2BkGyECvCsvswz%2BZASuEBPuA6RuFduNJx6zQQdoA3S8ls7sQmz2TpEpl8FkzgVnjQOHN7GA3WS4957T4TXMspoRuO7DGv4Y7qMEzpbJa5%2FacoApQ6wjMro775d7naA2&X-Amz-Signature=d3e67eeb39e47e1eef453cfaed0d89f6155a39ea7fb51f6bd47918a7bcb756e2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QSDNNWZD%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T080115Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIG4q1BT3AGVqLsGPQcnTDXXpJ5DiH4uEXi82sdbTnZMKAiAhKVB5H9DES83FW7u4HSdh%2B3uKt%2F%2BT%2BmRZmQib3XGJoiqIBAjo%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM7Nlyg1akrzgS3358KtwDXCRn6DtvQkOcM%2BnshhBK429eCsuuSPX0GmUakEp0jyO00LXLJB6f%2BW5E%2BKWhelLYfa7var7xDwXaaMBqzfKu%2BfEb%2F4oVmQ9%2F%2FVekfhjf9oQ8n12Vyy2X15GLiIHtqSZ13Mecz5Odl%2FIJ10HxChs9cZ9MwDhgcU1CQFBP%2BbJbPuuorddgoh%2F%2FHzHvbBY5j8q8TEnubLi67kDpFsT4DkRaJGuiVffl2AIl7Hv%2FxmWYCw7XdwEWB7bYZ7WN%2BGH0PlCPy2pTDSSJHBKLgb1rYX%2F7fsuI5n9A3ybVcRDvR3bO88YVBB6vBu83oPe%2FvEBL97H%2B%2ByjvncYrKNjGz0731rSBG1BlwKY2MsUdNkN3mcrnKkdEZNQhQu2V04QOJRr9tcpHZ5wQjtIG6mVN60P7Zn3wyOum0uQbFkGvCiExK3BdP2B9MvX2ugk4FF6criysHCS0ZN%2Bnc0%2BwH8itrP191XannK%2BV5e2l2oNrApK0aoBoefoPO8uu8l1bSNGFBmW%2Fx7PU0cGGRX%2F1aD2etwPmEO8NXADAARkwfVK0wEYtEuVD7KOZGpdYdvZLR2kGtIvhirrypmYdiQHr2glKu72ddNuVO3fVHTojMrk7yWR6MYXZDClYk7zN0Bd0YSdrrDUwqdqixwY6pgEqTuApL9B6KV51RJIWzwsHMOjkErtgAG%2F9gIVMO9byujdo7piEGwyxgyJxIQ%2F4aAPYD8lSb06PG96SfWATyyO9ky8YWTfbngHe%2FZZti0pElQT3UNWoBp4mnSqGqbniaXz8%2FrsAfSxTcRUiaxE5AeVdmfZdV6GeKavlC%2F4FK1luWcXhJ5FS6rz9uvFGf6X1uv8ABV%2BjohLDl6Zz36gHgpwMgWRy1eL3&X-Amz-Signature=2e80c27eff44983ec4137d4893fd56af8c237b58a143bb9b19b013b93e8c3c68&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


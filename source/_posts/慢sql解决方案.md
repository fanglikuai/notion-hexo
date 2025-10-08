---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665DXI353X%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIDOwldS7dI6soJEEVPLBsOOtK9RVzH79ENyQBhB4Z2%2BkAiEA7mWga0EDEnk3jf8ZZPjHY5hi9EiVFrO9wNyZ4HS0iUcqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL2TS9mXyWQl9%2Fe6mircA%2Fn5ZRUHWq1D4zSurvtsRj4WZYzqrk%2FH67D9pC3R6mFBQzoLRCNdX47fKoGEoPJ5aznAByGKtG2zWBD1%2BsebJdMgNeSJ9vQhDDbh2m9xehlhWTdzmzqsx591TY3YVnFHXWWMBrxJOxNKYfklPHGFJ4LGRQzRvmx1axM3voaypFXHByl4RdZ%2BZk8u4y3whWOc4jwTi%2BnU4Y3W5%2F6wTmHMwuJtNMEXvHeW%2BUUmMCrMIGFk%2FmRhRIWpETBCRSokhekLgprKqFkUBV%2BDfWeLOXJMmftx%2BsEbmmKb%2BfswLNXNVgHf9v%2F1MRgbuqB8xBHnokYOgZB8WTiizAP0cbgVlF7vLp7eGTC2r6DxfbOif35bgQoWpdjgdVSxLcHmvshi%2FqU7hPzWQnV3EK%2BfccuuCM3uAGgTmFU5jTaLu9qyN%2FucnyQB19%2FSdnU4HxDJ%2F9fA58syA5nw3eTy5rg773z66ETQaDZyutYhe6G0BJM6TIferZN5OET1jKWpnbSoeRs3HR9janpqF4JlaXVa1LGcEvdr%2FJSdQ9k0vRQer%2FFulA3PVILDs%2FFf2ckOyLxSh1NVIKtoeVp7VceqxRtjLr2BhElFoP0FGqL%2FhC2pVS%2BGABV8DKhFLh3U37voh93z8rKJMKiNmMcGOqUBZyiAOU62%2FdvWJryKzcbTvMPHo%2Byink8JZNn2o%2F%2FKOZ90My7EfJtLtCOn926gk4Y08HRsR8wBh4wSXvuVlM09%2FXpGcwcWJv4eBzZUR8R%2B0h4m1hcBq8FrfW3qbEF2ew1Hhqqr9vnvE1e9gF9vbWhQLCWjswGbcO0lk%2Fu9KlIiQ0GwDH61aztS2rV3JYqFJpR1xeSIDdeCqt7%2BM5DFox%2BvrMigAJrW&X-Amz-Signature=f0c02fad5fe5ce4edf10a6e9067a503c228374e262b261edd37bd4a806b3cf5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


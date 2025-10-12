---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VX24DCEB%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T150056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIELWZC4ke95F4eMszaOpsLbZXUUBvrUNfmolEQZmns9WAiEA%2FuTnnnVPHTZ1BVphJyu9Zt%2B2W%2Fo3jhYsTERmxnckM9gq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDGNLTQjZJyF1ah1u2SrcA79Pmnt%2FWr%2FWytEzuYb7t8%2Frq4b9grr6ZKOqNzTE%2BvXEex7Z5D8KHeISgyh5LLKR%2FU6Ow215gw3fk%2BaSVi70i5v80Qsl9USKu7kt6VYyH%2BA79%2BD3BUEokUi0XyuqRHz3lznZzdfKNzC3Crl0Uf7ufQWoQNkGXIJDyXaSzD8Pv%2BuzkQKNJyJLSwDJl2bIu0wK02EHlmo9QEKi4WgE1nR2SDUEMBJjO4sp1dSm%2FLYeKF2ADPQByRBsaeo%2BoA6Cv8Lv%2Fr9qTXbDJTqA3%2BnqR0PhrNfzZdXANavmS9KO954R41nnqICn31pI47%2FhkoEqws5kEygY7rxHWecVf8WyQv23AHzwTmBH8ckuoAwWYPtkR520xFIqs5pLoNDFLKLOpyTbq5cr1zA%2By5XwInhcNHl%2F%2BETOG2hwQ4qe1rX%2BdK53e%2Fmv2g8PqESVX6A09Z5H90lZJaI1oar%2F%2BmfYMyITzW4XZ5KJw0FTubvqa%2Fr3KuFsgbcIrrlesplhS2ewkexO%2FAxwYt3CJHyqYEnBVFO%2Fy8JFV0NS%2B6FP7FWz7nWp4VCnTShvaiD04OB%2F8YG7XvfXyGgN1gV6BsmGALqT9PiQW%2FGHrVQmfATZp39Agk4y7FcIos%2F6TY1Yx1GEQRLTYNySMPS4rscGOqUB8Rq%2BjFD4jqIlfkYjxZ7og2MeLnv3955WUyGVhsaQsYvEZVQjmj6xh9szFOx%2FEvBNLGhwf1mbBa1rkpMlWDE70LvtDxmKvhbGZyqAw8Bm7nIuy9tSb81D%2FB5YoPmwsC86%2Blt3yEJqVj22o7mEs1yNh4ME6NJEbViIPQWCZmobuzz1RUd1Y2oIAn3nQrWRnBr6REqkZiEkHFxEJhPSm%2BDjdqCbPgPo&X-Amz-Signature=71975d488fd5b7167a691f2a133cd10badc8dd99738b1d58e2b6ac8d4ff5d0fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


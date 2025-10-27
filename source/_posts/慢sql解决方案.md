---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVUN7G7X%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T000053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDOswfpNhUXYoLUN2x7HhP%2BFr2d%2FiuZB2yKm3n7YflXygIgEqD7cfpT%2BUu07xc1vDDoEVziJdITXu178bAMLmYCmygqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDA6Ib5cc%2F2EFPhBbqyrcAxWKJAFWFz0rBQl%2BadK30Lvw9kdrT%2BIRwr6SqoUqbPU5rq21cxxwv6L2Djg5pgy8IvtDPqNmAyK84NDPQs%2B%2BVV47UqafuM%2FURLfMgoUkaE0c%2F66V12WJtWOGVBGBdc88UwOUJQ6g%2FTMIxYOzMO2AT4bjoP9cD1AAfHrAG3T8SgmGGRjqZ8uB33PtpdIgDuiRtDfSPFVKtA0s31zWEalQdRwxyS2eBR8GLc%2B3X7iF1Fd1bx9k7hAkbq4IywRnvqge5VguO7ZgFoOAX0f%2F9k8OPmdjaM5E0Q%2BI61tTgpWU4nLrknN%2BL9HZ%2BUd4doDPKVcJqTrwCqCn1Xl9c9viiU0D1qe5XqnC2PyktdEIk2dpPJRpA0EKfrPPKeT70OsmwpO3dOYItSzdYbG4Pag1dWqedtiG2B72CEQzO3GrBqQO%2BD1gejNjksnGsGy0LtTNr52VZLbvJFMK7wDL6UNmAatM5TXfkCaLUV8PmmqqvaDStYjuQ2Zo%2BsVPCvPUWZVgA%2BvnYr9ak2%2FNifH8w2LS813TV7ad8VEA6Zptf7JwXs5eBaqEtTObDpScBlFFA6robRbmNK03Ugi8qR%2BgqBKI6Fh4UzTlfMXTrUHcH36E36XnIoavYnGyPV7v6A1G2aiYMKja%2BscGOqUB9RzySpvxwdpVblIQXXDpwUqPrqq8scxgGyonjoCl%2F4jT5IwdAY7xe3z3TUSr1zDo6%2Fv1VKvNviWS%2BR7vehj4ub8w2Uy4AVkM3Xfxnx%2FV%2B0o3ER%2BTAUKa1NRVYsAoDc1XuPPh8zSe6gZqm1oIOwChYpcKa4Umvg9r0jIO67e%2BFMtKkEkIA1Ni5VQNd76gZiJkUokNLpB%2F2yQ80UA%2F3%2BhtB6poflD9&X-Amz-Signature=0e2496d1a2440a09d13a9685d0afba3392fa1838a19108e40fba38a0f18d78f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


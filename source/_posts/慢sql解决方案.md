---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTNZZ6AZ%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCEcKgiwzHVGtzAnaSBm17WZbzrtqfbox6wlaJMVIeohgIhAO8xxsDGtle26kfjnOqQdIErnOirQjsE2hsYTbqurUyiKogECIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwp563PYWNwmfzry1Eq3AOi%2FeF4XUS6O%2BYeXxgxRp5L62GFQO9gWTQS76mR0cUExuQQJAkFH9HZBe5zNhbuSPwWPjOo0FaDnVOD73CfE3F4fNsbzXpfNjLmCJrLlSY0d5AgOtSYd1OJRydc9PGow4W9%2BHB8uehtn1qvf1izzZsVIXe6%2BWtpSdntj4%2FE7wt%2BeGDIhGpmFa0wvBJo%2FjQj1hvVz0hsPkHGVMMrld8GeicBZjQOPmajc5EkWbW06DpL0p%2FHq8bCnbirnEqTBirBZrAkYtLakqVzKTiaF8l7njgei0aM9tbK6KTRm8v2rpff8%2Beq3oorRqZemrHrBNblJ1Mz05OjTnAw0qozIqD1aB3pw5mYltv76MomMXlBNlR7DvGlJNhXIAGxBgFk1B98gLXWq2uOXviDuOSmcCSJd5%2FqOS8GdL1l9lSvNETCWHDCcghdkqLTL%2F7JB0LP5ZO0Gt746RXP1FkF%2BqtZhZd%2Bh8k62EbhsE3di%2FwgQ7MbB2Boyc8riMsq8fXF3bnWq%2BierjLq1yukJ955F0S127QnYJ%2FTet5CXffJ8UFWaLkECqFOqCFn196t9L%2Bwh%2BKtbyZ9WE7noPW5FbYaddtpVNvHfJCFqawe9GmAe65gs3%2BM6zSf4J5MXAdOqOU0HJ5WojD9xOPIBjqkAZ4YQBiBVLvhvmhOOUZI2QAZGF0NK53QuVS%2Bw53rHYVd%2BhdgITw1KHNev69OUGcvtbS6o%2BwWydeiHhOyX2TvF3kkzXsMSFobl0x1yvBBPZe17cpZXEljrfUnuc4NUorHnzx8x30Lm2nHA4T0hTiZNCgn0h36fytQXzGBNab%2BCrxwE4rhqMZEi9ZR%2BcCC7V1I%2Fq6chQEMUZ%2B7kbPkiG%2FKCSK9bOwU&X-Amz-Signature=cdde31ea0291695a8b4e3eb5ec348cffa7b1a959e19e622a613410172c9fbc84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


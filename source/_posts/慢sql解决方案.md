---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WXVBRIP6%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAeEVsrkhtWlGT3hr%2B4%2Bk%2F5gMir4Hc7gtqLBUiS7xNRjAiAYJQ9MzQE3uGQDDljcz%2BUDi1pY8ppjuQeJzOiD5jod%2BiqIBAiF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMp962XUSGUoNzTb76KtwDQxl24mFxw%2BTyNjbv4sXoc8aVjqupXzD2CVIXSbDssbjjk6W3Gt9Y1InTwkwfdWPkKfNmAA4xM0aF8WSzQF4zUow90nlaSNUT54EUvz5I1uHVPpitAS4Hgz6i2FdOq62E%2Fo5rLZDBUAwJzzQ3MXqyYwL%2FFpmznXuropAnoN5X2pzwoUdZMoIC3h4UOl9Utbd%2Ffb4Tv3YYYMmyjHW0FbPR28tcCqMlwwZvWJ9JyTfyHed8vELiT2tPqv8V9gSJHBTLyOz9VZDJi%2F%2BTNIDEDCPsVPOgx2ItpLm%2BTBctB3JrzsQdbMJNBvAub5zMOzlY4v7zfw9pdUjXiSXRZvCI2bz0QIsqk6ULL1UBNRom33r%2Fx1DJAO20UXBjagBQsAhH8%2B%2F3slu4leVjCTO%2FvKqdMKXqVs6M4%2FHFgcWrFuTXdfs3zu5%2BtuCNS8id%2BdtGtLjM%2FUOAs9yRRYk1T4qHSxCKysFzGTubxGcxUlf34Pby5jbwYQ%2B2r%2B%2Ba6IYITMSZ8%2B3%2BeSfJEZF4dVwmrtxgWqUuMrapKLnQa5p2DoA8hgORChfAIMGONNhRdfFz8Q1%2FAaPtnTowIqHXpK273WCufue7qNmNogJZD0t1Wak68ZoOr9LeaJmSOu1cn7WgWpIEY5Ywv7zjyAY6pgG%2F2nmd1qJyskwX2TBAGgO6%2BIbEMkeSXXEjX0Z9dVmTdEtWWzgCFXfha7hBEDA0qZMatIQ7LE%2Fv1nFbUfcQ55hKgSSbYGDrkuNCgsNjbOeQPGkomhzVbwxKET7y6hoCfgzaTAZ4h3chIFhW9BQlIQe6UZL6j7lcnmUksA%2B2pAv0xGFkVVT6ylT5%2BTcl3NBFOJ3mfgO83l8l2KatVC8p%2BRRmPP36K0D5&X-Amz-Signature=96f15a10d566e9d057b5855a475168982654454e9d3647a14be4a059b72032e7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


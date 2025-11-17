---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YAG2ZDG6%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIG2HbvKM8ay5NJKRAR2neWADA3QxZ4em%2FEspgGU1BXAVAiAdZSM7KdVzZH7ugxW4Z9Dqcsumyz2WGni9ywCTeTDRHSqIBAil%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMO2w%2BBc0NQMfnF%2FPAKtwDwqQ1XMPLBE6YRnSVAYM8j3qoRl9rn5IY7wDdnrTRRoy4IbabzwCI53BZI9tN6%2FTjHaupDsUiO8tyElTyfQZd7Tp85ijfL3ysQeK6ttPrFcDU0IqzLFi0ZDozwPWRdeGa9Y77Sugpxk%2B%2F%2B8VVBwEFN%2BRsNU%2BNWfLVCXfAA%2FksP0DR5NDLoJVJ5VkQEw6BjcT%2BwanNXMmM6Q9V%2BCI9LCihfDmlTnC35NEUUQRYcs%2FqDNxVCAOe%2BTM3Dmhcvuh7HlKhy86FeCiIB9hKEbP60Wj9WIH39oe5xfT1s8ELoq31Y4UtPgZhKHrpJsx8HTlFSkMQ8xXK26NLSkCzQL3%2BUn1GjbrPHdC2MV0l3vp6lHlqPkGG4Xsza%2B7hClT%2FVKF7ZddpyiOXjBoChVrFL9Da9PFr9V07HcU5SNDTS0Dkb%2F318p1hwnoZD976XTSKHxDZ5E4stBzjLlp3xy%2BRu5yEJV%2By88eTLf2fNMg%2B%2FPyohy8iEkpqF1JdIUX9JNJmYqch4u%2Bjl5Zwr98dimESpQ%2F9cijs5gbrVQA%2FDzGXPAsicGgch%2Fxg%2FB2cbX1oY%2BDIs76CsND%2FylhEEmpN%2FsY2et68sHxIR1LnKmN1U%2BUTty8wiz36aCPbT7W%2Bn94BMuQtXDowmMHqyAY6pgFqr6EY%2BFrrx%2BBuNAF%2BbangAmYrc70u%2F5e%2B3EICtEN9LgCR%2FxXxVpM9G4gtkddIGuQKaicskYAyKSDXU9tRvJoKHyVqTaxwIsCpVzHllzQhsXPVxYLZn78IFCw%2F5hOo7hpRpqM%2BBAtxt3zJSDoVRAA%2F5e0ZgtReU0PirKY5Dp92XAWRwH4BgHCfpisGCjHogz%2Fi2k8CKNyE8ZTpC%2FHeBLNPJkyXzdpt&X-Amz-Signature=ee722dcc9b4bb2556306a456aeeee61861c8ec88b410a0e222369554bb1cb6a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3XNMCOI%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJIMEYCIQCt6%2BBA76M%2FymIKu0jH0LTSkai6m33vieNuu9MUMI1mQAIhAPainWBSg3hX9ompSCdWKRKsnFvRd%2BFARBzhnSJsglOHKogECOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzGDUi%2BQhMN0JbPFV4q3ANrjyq95t2cnbX64OoD21GKAwkgayLJK2IVuoDDor3%2FuwISwmaSM7LtLWUoyb3kKFeJzH3CIRNOBi6EsKy3jU5A9Nn3cqhX1BQwbuRHmuQqws7PbQFUYH%2BmcqOfYj4Q9w54AWoFh%2BQH88I8FpMmMvbSfng8tdnlRMHpjx4nQe6Rm55fpigVki3xvra%2FalqpJbHFBghhWqa4qVKYSPIFUjWDyoqpptfVJcbA2Gt2ws6vACrvpE3umXWZikw%2BoGSlDMy2sA6UFXXGFmCsfN8blNdfGVG1DWsKCFWp2FSIQ6X9ileM%2B6DL5iUoUGxKEX%2B9%2FSTGtlQXkV2Qa9YUnVMgiDYYoFVapfZYN%2Bwb9gUdFXCeT%2BPIRfHprEV3%2Bwg6lyYFjYTSkRmqNKo%2BL99siOO8cSj4oVT%2BJ6u7UsXyJTUmqGPxpSm0MODrf75lM0gNuca4sHCfM5EKzCGErneqQ6KWDycdm9v1tHwHfSUw%2FAdcGqjAw%2Fg1eRIlhRi3g83TnvxhOhDTs5Gp49rWCiuC5i0hlY3Y8mfQC%2Bq3NGp5%2FMLc%2FSJ36ckdBL0UtcPJa3QYs3kmYRSq2sCUsUxYBY6qPwo2GpCLl1Q1KLoZ%2B5yBeFzgt9%2B%2FycPZazFO3m5kfzR9fzCg99XHBjqkAYHBwEDDRRVl3BujkkGA2VzWSWv6eoHxRtz1dz%2FPsOmhcXvU3RKJbZaErfqjO%2F%2BCECAliuMsSxvD3eSfPkP15%2FwZBSVQRJVDAbGTOq%2Bfqi3CWEkDePV4TuX8XPbtPn3iy4uKxktuomXTClD9Gtsw6wHPtKI4VANHvc3Qm0BqP0iwf%2BZ%2BHILR6ZInKZeDl89fqDE5rdYXNAcCOlUkz5x3lnLbXHqM&X-Amz-Signature=45473d5ce70e2ed8f039b6d8a7eb917b6f13f84c1f29d4eafad15c87671d1cae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYTDKBKZ%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T120043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECsaCXVzLXdlc3QtMiJGMEQCIBmi7QA5XgSUYNSVhPCPWChmLZp%2FvEzbsArvZvoQRbn4AiAojstQ9sx4kCXR%2BuAKQtgs%2FeRzrncTdGyabv08wsPkcSqIBAj0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEWj6B0ZdbABkeA%2FRKtwDLTK%2FfBLIu4Fbm3zcQsOwSh57nDzzD61zHajMI%2FjxUre00Jk2TGWcL5sOOqbTU4Vxm66vV%2Fu7JFqN%2FKUsJyagvct1PzWggfjEP2xtNaAO3ovO22mGH7M%2FAf%2B8Y5gbQom7OqAYGp5Xw9kwFhnOWwYG1yUMztoWPjYWG92yI5ub%2FS8anS3xA6MrVS9iJ2EXb2KEs2l%2Bs4xBdyct3srzBUDWKFY8erczFCLz6cJ2z62yB%2FSL4ZpKWXgAnmTZ9%2FdXZS7vHMQ35oogitbovyQz35lct28Y5QRViMxOVH8lVpzFVdNLTNg8rGv2%2FBbLUwcN6pEMiJGi%2FzWmB5mpAsEMVYD%2BNzD%2BdmFl2etFso5ecgXZsAhm034z3QgQ53jUP4xS1z25kDCa4C8Bpd%2BIrTSMzwT4%2BEN3QFxuvv4Y0Hfb02BcJ9LQIj0n7gJ4zZQmyxOMAJU3eNmqxeGBnmWVi4wpdp%2FJVgt36Z6FI6kHnJv%2BAflenIk7Qb7rYbAMShoiUHy4AZmuYrT1R%2BcwWXO%2FaWxT9Qudl48tV2gptwgzbhYDQSrgJeuAIlKQI71LOzaxmlocHqEiSCfqbEmPjneX8YW%2BT5SJCjXvKtNcEqYo%2BvOzE4z%2B%2FS%2Bm1SkHG%2ByKbf90%2BIEw9O37yAY6pgFeudeAebIN1I15ZnQgTK1usKguaT94zpPrTwyWrQVG9A5vuqwxN%2FcGe14eG5zbWrNJ2YbmxAwypuVun50ffUdqIFnrF7x%2BJCz39nk1fUyiCeiHqMKM0aqKp7kD%2BfJcHNUYv%2Be8MDlKKOB6NqaAE%2FEV6fTn%2FlgwTGFh6e%2BOTU6ihZihOc5cd7ZRLoJGYUDMWxhf%2BAYf8vSmz%2BJVo1WAjGFqeuGIbe2F&X-Amz-Signature=f0842c92aa1e7809d300abb936f006648b311eaf0d4bb7b564b569993c35a153&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


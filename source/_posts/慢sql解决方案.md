---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FSQHBU2%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T170056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGJ1z9EWqxcGHLLGBqDhLUxLMXrMiwIpxqRfgLc%2BsvMxAiBjiVjQWzzRkMOS5C5ZzKqJOYtfzjDd8PWYs9qQxr1YQyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMNexi1HtTlhGuaPOFKtwDNJYwk8VrruTL4%2FJR9MFm%2FLBp4TrXhmJFQdo71TjyKw0P2AXx9Uvr%2BKkJo0JemqJtYV6QZHL4NzMMY9aP20vICThICCldvL3idPrINjrJsEQfw%2B1DhR7ZmruWyDjMD51ydIFXV%2Fzz9GPtgBsV6KYNcr%2BLWbRYUg3c4%2F4rGzLUUpZfGhgthfIqwiCU37%2F0yRBZS54ZKNEN9VLPpUrJX2oJpWyC1ouVU%2FeLgS70mH2uwO8A0dq1hVPQRK2bZYSb2E0Ab%2FqvK6P7CWMGS4QJgydtQUfV5Wr8Nxy9PN72WVphYppnjMnU0zS4Mh7n51vnmIJpJ5dqnqANZ2wZ49wTeSY9TMxWTMc%2F3yZ2QjqRLmaTnNVX1AqHnwF6YarMb3raaB4sk8z8TQqC3kNmFecxEIFOBUJkNEEb7fK4zGO6UrvlykFMP3LjUQpP8EcYqk82YdtGPNxwzdC0DWeUgBknODtNzj%2FKqt2di18AzVzs8ZQ71dHTg24%2Fy2b1b32TMhA3cPklq6mK5pRMsh6gsIvjEPwJ%2FkT7szFXMs8FsViSMwdjYUqh6TXnWEhGxPWmrFUr%2BvPhMNtqwiEoSbUWIqi4J%2F%2BP%2BTRkqrhwHAPurYeJK1ITXM1gWUQqpA4mDMNCQi0wjf7zxwY6pgGMIJ3N%2Bt1m1%2FpkUmXpx3zeivrAa3BYgZuLBv7TvHKpZuTJoDI0CXwhNu6lGb9IIro0tAimRtbAPTKm7DeppnJw17uXfFAM9jQDCRJGljNm6vx7PlHTZMh2GrYb2OK5YUx23jm2WZ6c0CvH%2BQShxMDchO%2Fg%2FYghoq96%2F%2B%2Fb3LZ%2F%2FWYDcE5SEr3XNkK81BQHARQR0ksDyHLwkzbEJDyTiS867tB2Bv1%2B&X-Amz-Signature=0896163125a8c15dc57ebc64872ab30dbb535e5ad982b167918d2c76685a865a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


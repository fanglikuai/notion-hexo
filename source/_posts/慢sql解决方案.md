---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKOPYYHO%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T060053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAdJ5TktI3dBxXdWNeqwb6lKAXD3laSskYAf4ZyzGwzcAiB1Hxoh7b1dCGro3zduyl%2FXY4fqt%2BABW5dhWqk3xtH2YCqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxaRsKPOgNvJkIpnuKtwDYnH%2FvA10YvIKrOsFpsH8P94Ev%2FuoLGURGwiOKF6%2FU7fqt31%2BnGiFdSGEviiUnpnzbTtbIa%2FYb6XXSxVbzowgtqrDDnyrFdVgd2zInz%2BcvOuzGvBnvbl3NgBzvI15cx5scpWIQb4NCv8Yyk39wBxkYFT8TrWrz0sQ%2FrW9p02PXwEfqLmIAo2jRtRnAzM%2BM0HlL%2FaSO34yEVWoG1JwwytKeT5YFD%2FCc%2FdsUOoK4SALCqPd0heNfBqvoGTKlN3ab05Xg2ahuVnTMLI2N0GSAzwbMsYhnJx9i1kPjTM3DlA%2FSHkEdXs83NqpDe%2F1UYa5KOEdVv3gMWHR2bCzPtWK1XcXCV0h%2BgSuZiM3vncSVvSR3OHjU3NhxvYUeuiNC%2Fqsif9zcS4aZlHj9uU5myVj9GOAvvKOTmur%2BOSIc7P%2BxNU%2Fo9Z0ioeQRR5vocs%2FefPZWOJ2wozv6go%2FCsMkMcaLkLp1Z4dU8FYWExZgjBONwVKpob56hP4ExZhfrO4YzXpifxUBmy8LFtvgHHSaOAKBO5F6GHIDLxaucglwpbSnZoLNCSiDUcQ4LowAZzZviOGZbTMXL5gZyjQlrb6R7s4hqX1%2FZ0%2BVf59hJ09qmKRuBUjGUnhqGFrBnv%2BMeT5YmGIwi7ukyQY6pgH6yBZvdR59ugreacnIIuFUaK%2BCIsiK%2BtV6HLB1jfxiYJ%2BJ80xNaWt7NOd4CV%2F6%2BGlCNikl6JGg%2BDhKV6Kp%2F8G%2FfS9gDGY6ZjWBlKsetSAQHutGW77XLlSUjHA5RlQ7u0kZO30xWpAMc0uEr7ODTSI0%2BddbnXSQbiqvMwatWKvmLQU5YkHKpxyM1JmfkrfX1NvABZfl9rovQ7zG6Oq%2Bnq3sr28eUrQf&X-Amz-Signature=c8759580eb086bf27002018abad741054ac80ed460aec7d5301c1c47411c6046&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


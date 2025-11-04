---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5FBMHNQ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCqC%2B908QnUde%2B%2B6UUEd4zWUD0Yj4Px%2Fx3Hoi3Yt2gpIAIhAOiHnAwiaueknly52ZUSXTdMLzrj1eL9sN2ExUIfp9pMKv8DCHoQABoMNjM3NDIzMTgzODA1IgxMZhHmLUBCogB4%2BFkq3APNirZiWjyVInFM9hYS%2Fn3J%2Fb%2Blwt0SeazCugndICIZwWVw0CmZVQp3GdVsUpQ2Mpsb2Q7nJjcMPi%2BxBGurTKjfoZvkxImsjwCw9FsnVDYaq3FzkanLqw04INaQuj8KqRspm5aB3NiN2Q7BXH2q2%2Bw3Oxfq0SZ2Eh%2BQxlVj8MLDpJ8UW1K8tGzxMDgSDZWfVNs%2F2%2BuP%2F5sZGv3oVoonisWEXNLfLqvmOK5rLbQdHXygNLuIoeAahTNyXAN29MeRH106Kshdoxnd14WFBchM9y9Szn9CobyiIuLUdaYc%2FRRTHEed8l57F8cbZayvyEDfgTuzLKutXQwhT7yzVbDLrwHWVvxaqQbdoIkqX1VCWXITZLbkpEq9jYKTOChuL0B2zIZznfKjKn6ggMytsIy4ZvNig%2B5LVx8XOmDyHwb7Nav6N%2BjD4dwwGBVgDhF47To9UkAZlj0Xu8vP%2FqShv6FziP5wDhqV9ZzmtvjF3Vpso%2Bsn7B%2Fe0PQyPCEvCNZbEudnMREMmC6WCOfq9WiPc2tBtdx990rDtuTMCc3pwGpnyGogYhawB9XpHGkFcfXeMbQ0kvLbeUCsvjSigGf72bKRTi5STHkyCTun2InMw5MNPGMix%2Bae9ELhOTSFYR1DdzC%2B36jIBjqkAcTAqjiCxGaivj%2FQNvWFcLEHOgbj%2BxYf6NnciFRcKishn8jmY33wMsFz1UncEkLCYYRZp3fakG0Uel2pKLSrV4exdUVscUaVRLLpiQSrPah4NdPJG94RUFOxPO5EplRIgsmT8uHYN2JMsHVCJ0tkKs1BE4zm1MF2%2Bl1f0wgTFWljNy0%2FGZW5vT4pvCqRHlVOzDXryRU8qidy4zDWiLM%2FA4cK%2B3hI&X-Amz-Signature=69423c8cbc2b2ce4c2f1e3b89fbddbe56bd4c6e3dacdc5a4f8a4bab39638b224&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


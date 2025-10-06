---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDBEFVER%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDw49VqDZid%2FY4w3UOalarWnTxo26SKVgc3BEHsPHxSpAiEAz8HjfoH5SvfcEWPIexvC5inV3h0iy9koYU5bzfbs7AcqiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDfvXPFXAL0hOt2pZyrcA7HLCi0B6IJPRtXLBQSGHRoiVY7nCN57fWoeBVyEXW8bxN%2BgurPMC51Rst%2FH8DZ%2BIC6GJgMUvuY3TnO8yokQPhQdb1qLVU1xSU%2BnBEyMX4tEqdUw5xrdr9VaRkTyrjWDfLX9u%2B2ooH0%2Fdw8IPLBpB1%2BtylH38GAUXTKm8qVuXd4fOqvdPIqcQqYMa3z2d%2BaeumYERZtUzphElrBzQH3ohmz1zPl4jYTgz72EL3FrPPZC1vzi%2BZAtw%2F955bFvvx%2Fp4pXp6BGAdnPRYPa3zq6PgoJ2MHCQ39SM6Z34zo%2BECjoSF26l11Bx3kcbYuSs1R%2BgpUeHiGzvWe6mJipWusCy2TmdjkAMm0WS3i9nrhrbe7I8rvOKUUx0qIeXL02hyEG2EJwIAIPNyeAFFKATDq3ByCs2fisjz0sHod9UKTd4C1roGmio0z%2FVNqPLDCG4cfMdDBe72Cd5QU%2Fw1Kc0a3YdenJJyR%2B%2BvuKsz8owV5LatVvooQGFJyxkAlvtRmfDRArlFbKKMTtLTMLQjaIKVytHYtR5%2F%2FdCZYuGYa35sz9O2Lzp7eCfZfWmK1ffljRHXWODIeosE2dWIV%2BDfGRaJUag5UfxAzDydSmDn4rURs%2FvZlzflFndCbGp%2Bq%2BmgfhmMPr%2Bi8cGOqUBR%2FL6u7NW9cK8CDHjKec6umIGm8S3Wq8Y4sne0A780uswrYrKSfuvUHFoCGVO9DhEHbryuuvZVdaKknysQIE78qxTPA52BRoGSQdl%2FTwzMHj4BkXJb2Hk8vRWUFGfqlLEcA5cvDhzn0QjZVCaLYmv3z1HRh1dO%2F0E2Mx%2B41xzVc5bFvNhpfnsnWixXUPjt408SjMtoo%2FtzKUWqxGtPFtdfjb6jEPe&X-Amz-Signature=9804cd63614c35d1fefa688064dcae8161e0e49f62628e3ca3f8fe3f56200562&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666LPFOUEP%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIHMHvp6WrLV%2BbYv2bNc19yCboIEXP6yG5IdizsEjenFyAiEA5mYsEAxqHnoFFE9bsBy2upTMvQx3npHl2PKnYyhjrxoqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGP%2F6sN%2BVdDT%2FhASXyrcA2%2FuD1T2xVY%2B4PO7GM5B6HvQfoERlX2WwZoVBUIEoqEm0SwbxHIgDtsuaLHnT7x0gxQr9iSeC1A2IuRJPV3l3u0K7b6zXwrhXkcdGl%2BhG5lMZXFb6voNHL6tePlRZ7hc2hOdxUvl246bvmLsFMfMbaP167oSks3QomKl1mFkCvSMQYQGaoDnu0pTsndzcv%2BfvtVVnEdWngr%2BTamnHPfOfe9vQog6xNUSN7zu1Q4kbtbBuOa%2Bxzs%2Fp2R6Snb5ZMpxtCo7%2FXI%2FtUdTVCNaiaRucBJkFFl%2FHWrvWX05SkPAL7nRQlMxL23smK3EVIuD7TSDByPgDMUGxlVgEyoZC9Tp7vnXc3xtg4EmD0hhWenU9S299qxJ4rrVXTL4uVR8PRqKoAK4urJSrlqEcBVni31pwkaQUiCEgfa1G2T7Kgs5p5cEYDjc%2BCalB7%2F%2FY9sUQUVqi3mjeBh42fHZI3JLD2jzUId5LdJ68%2FrntKl5gMjo%2FsDP9mcAJXnwvLiIS3J9W%2BE4w026cK5l83tpyP7eFJimyCIOQC8j6cLOERkGkz75XZ3GSKmPlDDPG7om%2Bsg%2BBGq3zSrEzHxmnU5AO4MAEnqGu%2BUeiMcvxsWTN2jprggX0lMldp41QwSmNp3SfG8aMLi318cGOqUB1RDS%2FygmhHM%2FZtVx1aGjvirrG5aTo3sXnZ1Ps0hjZB7uxrHh44yOnARRJYtUDO0kIdOYB3MBlFXjfFXg8r2fDtt0wiuq8BQYUf%2BPXrVt8A5zKRsM%2F4fgM8q4L5VZIxWHDI2wA9JbdFgnfrUNzPPEPN5uJqBAu8A1nC1%2Buv%2Bb5dsO1ABSh4%2BREgXfSTyU99QTbrKd78XhckejMyfijNaiqgOinm7A&X-Amz-Signature=566522c55956d30754898fea18db50232d50c0bb5dc828ac68493d13c3acd239&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


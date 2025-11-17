---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667M2SVHDE%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDt8%2B%2FclGcVdX1Njs1ND83bdQRNIERLyztTQUk6rdfsRgIgfx61T33tB8dMyooD3EBz1%2BdExD%2ByecQS1npj8upjb7sqiAQIrP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEQuzUrkd%2FhTpyvMvyrcAyl4FS7lDvd7JoBCUKJgFiAvdbId%2BqQmTQnDoRgvoivjCMi9lD8YSzE5WPM5doLah2um2QQqtAZ%2FgdIPrLzgW3WPi16W7SDTnR6d2Sjoydq5H0qJUQd0lfHkXdvQ%2F0U%2F6xm2Xh3Kxs7%2FPbZLSMgL3CTF1rQq5s39SIbSjex86erlJAW7Yw3K1OaBBOHf8zY3u8J3gc14u2%2BcbP%2FI%2BNY2JGg%2FLWdjQLPXAbHT6w1rcDz8NNEjB%2FB155MtmSdjCv%2FBXqS0XA0sdKtMIgTWIWF%2F4dAz%2FxnooU3b05H6kGUqLo55BzqxXZZ69QlOPq9vxpeLOwkefV5z6SccUCDbhELkNcUFFNIrKenm5AvMCLyFdkIhYKhwtb%2FX9LtVbm0qpaXb%2BtwS5o5h26tTTQcusxAUS3A0lEtnWd%2F%2B3zg7YAretlQ77q5u5qiGmdmFrqLYR02u%2FF743Hs5eDwEd8kH9CPElZVsPoOKlPQDxkPLbA74jKpbBUiKQITZ%2B7lwJIPqN9vGuMgxdQEhmNWBAQPl2soO2y67Z%2FTd6Ed7vER0y%2Fabgp5dybpJDZG%2BAweKfEijcRtCBR1RlJy%2F6J5V2vCTyv8NNsTJBsJCfVsMbHmysdkqk5QJdPmFvyn%2Fs0%2FwXio%2BMIv168gGOqUBy%2BrNiB6TA88WIZnsgKFb7swXX81tf7G%2FSlQfxdbZXhpbAanSXJuzDO85X%2Fy0RuL9fyzo3OuY70lCXRGwOZaRgdZBPCIOoq6PbchcsX9aKO2RHGDxCT4PAzMNLRcH8G9A0f%2Fa0vckVBne%2BYXS2wjHPeW58Xx9UwLisRSfDX5B18xniJwD3Lk7r2nCkLJVFKmvbMudIC%2BWuXAk5sscCVUVwauSdckS&X-Amz-Signature=f429bc5a17dff42eb55446b32a38b095781ff419145a03e37c00656fc613b9fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


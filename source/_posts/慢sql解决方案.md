---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665W7G5HKS%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBcaCXVzLXdlc3QtMiJGMEQCIDMDqiTYHW6v7%2FaZlcjNgFGoI%2FgCXRpfTSocSbLb1HfxAiAT2ufECkkrwZKxCYEIisVjvdtQZ5Ea6tlkNy%2F%2FCatvACqIBAjP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMeAaaNnyqPzLk4YklKtwDEEZbzSDqme%2FJh40%2F31brK3m7AiTVOVaGJpgovKbzFmluqD%2F744nStMgrQ7epd%2BHKsPdlgazdfGUJlkVhb%2BLGO49jnDXWAhbjx3eCR4trrWvBZ%2FUswfYtHtgQKFxrcAB6zO3vrzhaI5KMgkPniGKNtVRlEVa3IYc7BRgE4yd73Is6ejF25NLQZHmiXmCur%2FRHR9af%2Fc8ZS6ZSSMV6dGeTQ0%2Fz2U12lJxKDSAeDyFcX59CGY5GTT6PGS9MkmYKyiNHq%2BI2a1ZoVSQW4E2DBKYWreIG04vYptqcHBB473KJsM%2BCiNxcNSNdL4X3ybTAULVRAjvHfrwqKiscV2wU40Sxf6lVZgInWxpmtqKvwbRGEaKNcQT1K4Oru4H1N0lCobvUPyqDGaEx6lIoGGhE64d9XpS%2FqnB6O5FRivnyGdvmmrDjPZ9R9UeDD1IMeZ3J%2BXSYOA82GLptcS%2FqO9mzEngiSTgYgKze8D0SZ9sYpUIUFEqvjTxFlSa4RNafpGuCLvgDawO6NnIblxKpJexxcx3vLKSWej%2B9RbeFyTKXMfuDAbSYUIbV3PrMzkXhkXrnyHSSYdwPaCLg9WYwIV7zlCjxZnk5a3oW2gpluUFHyaTWvWYJSVo9umiSprRVrQUw1%2BWGyAY6pgEMua1lgdjTHTQ7b1HgcmRiLO5OyiC%2BLuri888cw8VVk8PBqAinNr3Pq%2BCievtNo0nWN8danVRAUXxjyIaa0Eddq%2BQTLj7L8L7oSCRtdIaKCVsRVqaSgQ%2FIHTKFj5TJHlb%2B5y8dh%2FRnAtyv5Yx%2FtANu1YpohobSYQcmLxHLocG%2BfyKtpjDWoUgi6bcZmbD%2B5zrxOh8PMkdooXOxpZ6q93Z5TSQWNO4%2B&X-Amz-Signature=e6050e0f4845106b8adf9d86465e14811b1a57888f9e7503490dc8db224a69da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


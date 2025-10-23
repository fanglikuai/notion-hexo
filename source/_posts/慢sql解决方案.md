---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2MBRSHO%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCb4OIVOVIceHBCqbxzfpy%2BslRaZ9g3JIeBr%2BHN3sOmwwIgUS1XODNu3%2BLMluO8bXJh6%2BvebF3zIGhB1CmkzrtKdpwq%2FwMIPxAAGgw2Mzc0MjMxODM4MDUiDPD2A4tIPaTxfu%2BNcCrcA2f5%2BVHWJaaXVCehWiP%2BAlAQ%2FuCUfrXrltKSA%2BcXSiOdZ9t15eHjQTZn9E%2BZh%2FdMxqfSuL%2FFTk9DhQMnfNBXlwPcn0DEwev26b8IyJVuYQBH1LYiU2rKFMJrkBJ8BTi4Miz1Hm6ihu%2FdYAwHNww%2BqEaT9tNb39b1m5cEan79FnodD%2BTXZMlLU%2BmlHkQJ%2FA2FWZOD7HoE7BVAGCqO2bPq%2B5EK7CxGRd8ODj1WJnq5iOfKN61N8kJxm793PvY8f7mvuLm7L1isvUPB4jXqHZzRgxlfeZvu9eG%2F2lqPW3djp7XuOTbxMFLq3Pg1LN6XcmEssF8mgk3J%2FTJhCTcvdtX5EU2E9JCE9oXdxlbAw3KXLwAdfrqBvuPfseDE4c%2FeJeMX56jyVHdbqgtG1ZjhjchekJp9GYQRQ%2BzRIV0w0X7g4h4zk58aqNrtLXZJNxnyxJHD6gmchfAX%2BYh1W2la9l%2FKcT%2F6s9%2BPZLkn6naVC0EYRv8blHhwvTOpMINSo5FEFrLYC%2FxuzpsFxg23F53gV80abVaqc9jnB%2BRwpkuFVHO3591v1AhEhyA5XE5sPJdsTwxdrhitD%2Bk4JrJXbHGDC%2BB04Gt4qJctWWvH4R9GOe5l96L8SMX%2FFej%2F54zuW5GFMPT%2F5scGOqUBrlrrATuM2Ne%2BnNE1Uz38JfafUa36pwjSuoBoLabjFuXPQObJApJrOfLIJ8ThSaxbtEPvop4iMu47aqKnvSQnNQEGRgTN9QkW13wEGQKLbgeI4sGnJBAC9275%2FJYG2ZrqDdeZ9jB6OmcIQ65v0TpJ5xEIPMypA%2BXPJDVybn7tp65DH1u2ECCg%2FoXwxCDq%2BhaYmZDhCjK4eyvFLpMS47jnmZOP%2FFpZ&X-Amz-Signature=20fb2512159e78604c90e8414fa933d43ca25157dcd30c7b9d91286a13733b37&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


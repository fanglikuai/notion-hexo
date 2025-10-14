---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632HGTDNZ%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEa8h42jBlglnvtPz6VfkFSI%2FQ2rOFwLPn0Pd3YJMeF2AiAkBM3kCGsSthckv7kaHL2A9gq5kZ%2BQFfQL7W2tJ7cvLyr%2FAwhkEAAaDDYzNzQyMzE4MzgwNSIM3TOJrFMTV48ODYGCKtwDqelqNSP07%2BmAT0GCFrpPbJwTIfhoaqu8NA%2BVsVDyVgbIcCbMPFpbv3LRXSX2NII6cZmYeNSIFW8x6DvK6XWKMnS1zZYUGjDy4wUF1%2Bh%2BC30q2wPgLBk7PxOCm9t%2Fo2I6i6cX%2BEWIpa7JDCG96nAIb%2Br3vYHH9YSB9xPeh%2FglflDNFl9redkm5Znj86DLRBivrVwfcQ5GuMWdfuFRWZW3kS41JPP11yD4dtxCYXVglBbSbUXz2E0QO584nQnqNVS5N%2BX3O6C8tVMaYxCGGdb4YH02a9u7Urrze%2Faaf6wP01vN88jpaBwrmj%2FzUIDK%2BgqBxmfvrtH2R7cMt9%2BEnOj1tqF5YRe2673Cnnhu0PM53i73tAYnOUn2CGBryhO3dR2Cq%2BJW66Os5MC8Q6H%2BFARxCxl4j3VBlyX8WBdtd5Hih6JKle1oFga4DX5TyvQvJWQhKSRaooADF7tNQN6ycLxCbxsYT%2F%2FJdBXSVw7SvTmj94qnKwakjYGto1EFJvwoLooqLdWajdXmJ5V4GdXM3kEWs1JzSbtf12pEwTLSWzDsf76ViNpZDqTqngKqTM21UTWZCB9FUuoGSYyNrFRbd21WwS7g4eGNaba4rVI3S7E1EM1JLUXdQu%2BHh%2FOD2Pgw%2FsK6xwY6pgEDgFe177UUxuM6Ap5GhHnxPmyM5YyS0V41HHwlsIn4Z%2BhGMbpowhz1iHwy3xyy86AFXtFmuZ8OEW0FoQsrI7gT1uOfnL7jHfn21k6sK3Jb53kDVqOu6v%2BqzXHkjMgCokwTLR1oMjYG0RSFyVxn%2FdWEguyY%2BbgUjMGWTQz7rwE%2BXYTAnD0XaXwIUv0dKRQzLMKUukG8uc78kKdFRyDNM6Djgm7z3%2BJm&X-Amz-Signature=0ec193befdead9ed045b68d17679dc68b3fefd948a57c2edcee00927ee0b576a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


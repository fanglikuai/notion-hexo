---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TYASKNRL%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T140102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDSBk4zWeQwo8EHt%2F2Nd2EByWaV7jqQoKTKoh9eMni5fAiEAs2quoktMihVxqTGh%2FWUcYZyS2wKzDgGINjXr4TTn8Fcq%2FwMIZxAAGgw2Mzc0MjMxODM4MDUiDJQydv5gtgl60OyAGSrcA6YaPbGSUlcBvliwzaeqBUoA0rztqCG7JQKvEzQU%2FKJOqnhUumoFaGcMQ5KZPugr506cmHF3qGpI4TScGgPPfK1JFi9ccxbHKericcawxboJ6Yyc46pFYhCxNWubVGHGCVoF14Lyes7L7Jl8zw3C2VVoP81YTDCEP72VmjDDdXpKCRfIxD2I3qluiFuLAh%2FQsZ2iKcAdC7v7nW699iu5Ens76eIWTn6P43iXoJnpWaFf4T3R%2Fm0O%2BGFX4wShaW%2BVyWW0SBM89i56N5EULo4Khat8kh1C1W5xa4foyRkDwn7FMbNw6qWEMlZie0q9Fh%2BIfEfhAKz6A0VQCvGMMio%2ByNe2I1aeP%2FeElUHvlt7Al4wjJfSXyHpSxx%2B%2Bko%2BFinTvXOsJyj5eQ%2F5J08eFf7DP9cEYNB3A4nHfl%2FTiyixl2OVVVCNKVzDXUErLgTAZ6VAhgV1eHI65efKhPGOU6HlULZXETK%2BR%2BbITKX6w%2BgFyYrytpu4vB6YK22ZPwppsLRGv7Cjbe4urpzRSsUcHoKDV3RBWV%2FtRuY63EHzaBAfejmyxOGtwjIMpTf5ehV9xkjnIITLOhE44%2BV36kP1k8XoxJxdV%2FXxMe7xhJRb1g%2BYBe2CZwdp7mT3R9p1oQtx8MNHc3MgGOqUBXZiREP3LoE1aK0K%2FZEK82nuGGDfdjUD%2BMR60Q3ixVcCscHt9m3fzzujCHSFYO%2Fvp4WDJDATgYsqtG1Tdw1f1BujAzY8ZX%2BI0QdQp9xLy%2Fbgbk1eHnawBeYn8DOj6CjbtX%2BfUMhUauNEFaycAOBtJq1Q6OMlOhrCGo781m6qZJqnP3DGq6zT5Nkfm3P%2B2WbcZ067tc0JRyiql%2FEygBN0zVdi4Rz4m&X-Amz-Signature=f038de1c397fec9c9ea7be5bc2c6da278b0df2796a082243ba0a4cd11794062f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WQVDGUC3%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAarK9EH1yp8X%2Ba%2FDeNZCB0fEfrNNb42WPqGmBHTEVBRAiEA8jM0Zh2O7OH%2FPRs7zt23EedPpI7HM%2BrMzhnAiam0UKsqiAQIiP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAts3fPJ5yF9H9zFhCrcA3hEB14SJmXG6IdknuwP8oDudNH4wmECw3eNPu%2FsUcjL%2Bb2Ue4Ma9JxsrpSq9fllNWnjyiFP0TOaUhm85qbX6pIfNV%2F085LQWLLjlv1qICRddj6HuZVEPZOi7bX2uPcQZ%2BLohDkNhq1IwXBFzPKCf4dNriGzxdyjPkelj9fN1E50kcBTUWsGJCfAnTy89ukwI7uEtNSEMwnj4%2Bs3Oo5HrYAa5nW%2B7O3DMKvK8gYsJWdY79wUO%2BHRRCSa26YI7KCJmlJi9Yoc5oQ%2BmnJzt4ifbmQpB%2BefLcK21sat9xlYsb3dGwIbS7UO3xPJYCfergvAogvSXvgelIVLB6s99WpknLoU5ZapC64wW1uMbZtS8pWqaggJm9o0v8M5Mm%2B8zSTfgqQiiDYAKYWMMl%2Fr9dKm7VqQeIDVRVpBt0Ir9o3Q1M%2FKlFgcEgSN9G%2B5DNWOPPODdOM4LnmfhfVUNoargcTLfbmY6%2Fpy3LKhMfIIvF%2FwoqeBWHLHXsfgKwhE73X9qcT5zEVhByLU2qEbD8WatIVPL2waw1h%2BDlcTaCB0lG5mCLV28hDyqcl3ZtiTvHecTYEIFDWO1bPUthWVPjfwI7R69%2BinGBJLpM88NGl%2BmOGDM8C5y8FOcjyk4rA4o6qhMMH%2F9scGOqUBZgKl%2B8Aj7UpGQ%2BBM0njwPdioaEcp1AgNv%2Bgdppba7zoB1XPFKIxmMc0tGMICM8CqGbxGUBTQjlIM%2F2gZTzqSZWH%2FQTCtfxaM1ZfnJ58JaatsVWi5H7i9x6AJRN5g%2FcgnTq7R8f1hez6CsihWets29yFHzry95gyjz6Sz3Iwnep2xJU8UAqs63xH%2FxfFTT6M34DNOM7MXHcFRJXlywVMbpbxueG%2Bb&X-Amz-Signature=ed1bcae3468c0195986cc9b8e81b1281791c572f341a4d18ad0bb14dff911230&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


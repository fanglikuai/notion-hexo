---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIMTCP5B%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T220049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIAUQLq%2FsI%2Byp8M%2F%2BNG2GFjSMgP4jI%2Fhq6kwLcwP9ieJIAiAAt54JIjX0P28IhQyD70PE7YDlIk4iPYjKtyueMTRSkSqIBAj2%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMwT8xFa8OjIs2K295KtwD2wz%2BWiTD5iAtK2OlCIJuZshM7J34HSZFW%2FW4R3BM59VWZC8Q1dJgf2zGF6bgzP6ao6KevSsPlPwqf6qBCiOpon2lQX47HyQNYOInjfiKX9VvY0vJZou7yXyUMYyWLsaXQNTciRgmYbCcF30om5MA7oPP%2BN0y3C12Qnx4Xi82Ex8T2FVrFi6J5FGGNvohLX0xKSbuqRER7yYHeqr0Ms04Ah9WfpTSL5%2FtWp%2F7n8Dxli0iAUdTPji10V7JkZwCNcYffcbvp1kcv6snXHjL8Mks9Up0T98HcPR2uM%2F123gPULnXVZvmkfmOVvG83eiN%2FBdwj7G4bK5%2FrzeC%2B9S2t0EcGdFFsK%2BkeOHvnJIlmzKZVQAMk6c7WBfmFUyCKniz8ykLj3K9zcDli%2F%2FrbvigM0gm%2FrBNDFR0xa7FrY1%2FJBG6TYuCIazBvg63ZpDGVWsCvF%2B6p0%2FBSGd6CH%2F0NfVdj1ZyXRgea3BK5WNGLMgrPopgwsODn8fmiC1NqpxPVGAnqfWhvwl0vVgjjpJwAY5LhIOI2zOlbSvpDOtiEC38HUA1T0uKD4dZki6rZ08esi6%2FHc%2BI3M01s8KrDBWxYdYYa2pTkgKusf3XpxEPWhAU%2F%2Br63dPnbEZVm6J5QDQJUdswhMDaxwY6pgGSFYSWYhfu4NqOelkaKCuOvIm3n%2FLuavmRhRKMwTWBaCmJ96wSoJVTEQio3JCjAiDRUr%2BaKXWGhUa34dkX5crhsbGFk4xWcBa%2FmuOOYtJTrKHJutj4f1UwwJfFmbFPTABk2mhXdvLZxduvbSIP097JyFRYrmj2vEhRh8KVUJYsmLMqV8f5O9xV5Jq7J7gPLhqmeRyFyTJS79%2F%2F3RIg1%2FhoMZBSXZac&X-Amz-Signature=7c550c3b0c7f80687d54b986c5cefeb34e2ae9277296b957df3f803a9e700bec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


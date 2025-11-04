---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XVGT5AQD%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcZDoAkkDi8YifUHBxCNMYA%2BjkT8YBSdQtAzIOUVOXNgIgJmmWi%2BsYCsa3Mle5u0y1UyTGQIY5QNkKLT1h3imM1g8q%2FwMIfBAAGgw2Mzc0MjMxODM4MDUiDE2%2BFL%2FQXxSB5UB6gyrcAwqDObrFnePvKqHiY96b2MC865udqCl3ldDmyImdhEpcirGgEfuBIqi5QAxjqnROWkohiM4DvfjLoKlbm0%2F%2BkQp9D1LuDkQKi7H9ORiaeRDhL5U3xAtjCAHgjVWY9vxKnSJVXYey0FCDYo194kO95jlFN1hjCoF8%2F0J%2B3CENhAWTMjjJPw6gls9HhNTS5g9ndIxeORsdNqR6rwOfW9DRGvRuYmvE5264QJEhIsK%2BC%2B5ELLWPtV5U76JR5dqnaGFWhPf1jJsxXsXGqTJ6GP2mfkVIXyq2TBBjZy4jqKpsHNeA5nfVJ2RY5%2FvefqweXjHxhY%2FDvsikjOkm%2B5x7iNMDiTu3WyWBXqa05LEfqHKpTuN3RnDdABJU3FDV95M3kQ33u%2Bfcg1R4%2BUbmQpGg%2FeSAH3Ht1A%2F92nND%2FnniLMgdGpHY3B5ehnmqdyECWNrVEdTc0tnMZXtb%2BU6zQbF%2BkFaL14fVMvOGla4xnNghgEAJV%2FUF%2BtANMzKAWL6ZBedlnsYEgzxEx1nL4Tr19l%2FL1x55%2F2u0fzYwKxfsDANsCJ1P6nRQtvQNKPc9g2tnmM9XsaVv8eKaxXZaUC5g9eH%2BAr5L3EeEGVp3KBcFOxDued2oVSWuAMfz5ZjJkK2HUWswMJqiqcgGOqUB3C5nfJiyXTuWZI9ndUuK42vlKbiWQ%2Beh%2F9ls8yaH1pN3feyIYmgzOKwO%2BS2F%2FrSsD%2BakELk0CKnXS315Gm8b%2BFt9ZS7OT%2BHJlXCYJgtZTXXvbZ0r%2B5caIG6m740YKB9l3QK6ftKskqn2Btu%2FqgLrBSxqGtnfRPezy1p4zQpUzjIMvbFGmNeEByC378HzcqlIR1bNS6JArpBULzJ7UZTmobplkTgF&X-Amz-Signature=2f04ec7d9fad0f36ae09fb485613d34e07e75cada33b33156e3ff201dbb5f968&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句


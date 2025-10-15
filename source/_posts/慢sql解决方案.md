---
categories: 数据库
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: 慢sql解决方案
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/c46ad6c9-c687-4bd8-843b-bfedb8d1eb44/wallhaven-1p71gg.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627ZQ3WJV%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T170050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBy75OJzvjmYHL%2FHrC3SgzRUGsyF1NQB7iyRI%2BIeXQyFAiEAvA3Lxl6Fc5AFz5x0IprQa%2F8YrqDTR2AffqRNddflcvUq%2FwMIeRAAGgw2Mzc0MjMxODM4MDUiDIdT89ixBqY0%2Bubk7ircAz%2BnwmLAzDlkI4F9GJymiA1F9TJpsvRMUJs0leVv85m5zGnTwUbF%2BClavVHClVv5vhcC5OY6MPFPeXyhWQJQXrIBU36swsmRx0SAz2Kfw6KVewLA3fFMvM4msX9HGHicrFc1Oxcb0ZWlwFgQuU7M%2BZs5wH2uXEQ74Ah3fX6mJQ679aGLG3AGJRuQqyVN4exh7k%2Fy%2BSPnzenTaJc6BrC5opmy%2FsAqoT1yBxkAwYnwi6NgsYnODTJqsYX%2BtZ7JB8F4BQtw5qFEc8EdG9z%2BAoz3GwAhWLuTm%2FEIhgcfsfWGQ1Ttq79S120OlFkF9VEIBAiBI6h1sizwEQA%2FN8uZknAQnKl6xQPN2tzZvwPLJdo1JhTQxkCtW%2FgRl94%2BDweuoAECNeOfr5pb%2Fr2Joqnzly%2FIetImYvrpmveSWI%2FWv50LccCXw0xZV704gUvlMFtl7Bbek1nSuU%2F%2B67Bx3ZvhWtVcHOqrWyd58ZmX921whsEKifv58woh9Gb9VARe0FGH%2FRHG3IR10NqP%2B%2BMXU8UcTE0rqciblD6WwqFaxvWrr3gCbxV2U9PbgeS8bEjLTRl%2FD2FW7iFNmmJrlFQb%2B7xi%2B8LjcQsuPfOZQK4lF%2BYj0l1rBQReRXxwTdqVppgwiYtgMISFv8cGOqUB%2BiOWNho7VgZWKlAjTXrf9Fe75lC9Te6cn2Lvo8GIvJoDaevptoqn1C%2F%2Fw%2FvG5YNJdrVdhKvPD9gyP%2BJBz%2B6ztiBii%2BxMTy8mVY5q3nITbwkEs29tsEcIa0NbzF%2FebqVjCZVh8dmfdSZQ3CYGOGVDNozImavkHhIDTLUfRtNWtxO19sB0TFo0y8ZF6c2PeaCfgJ9u8p7qFXosRsVqEs%2BDTmIAlT%2F7&X-Amz-Signature=1156fc931ceff4bcd4a0662c7cd02e4fd8beb6493c6a68f77b6742d93056e051&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:54:00'
index_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
banner_img: /images/8e8b49d1741d0fd87d6ec82900a3b289.png
---

# sql-analysis


原理：增强mybatis插件体系，实时根据explain分析结果+规则引擎


线上发现慢sql之后，除了改代码上线，调整数据库索引的方式外，支持热更新的方式替换sql语句

